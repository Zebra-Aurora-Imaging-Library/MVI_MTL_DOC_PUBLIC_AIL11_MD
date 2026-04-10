---
title: "MdigGrabSequence"
description: "This example shows how to grab a sequence and archive it in real-time."
ms.language: "C++ Object"
---

# MdigGrabSequence

> This example shows how to grab a sequence and archive it in real-time.

**Language:** C++ Object

**Functions used:** `MbufImportSequence`, `MappAlloc`, `MappControl`, `MappFree`, `MappInquire`, `MappTimer`, `MbufAllocColor`, `MbufClear`, `MbufCopy`, `MbufDiskInquire`, `MbufExportSequence`, `MbufFree`, `MbufControl`, `MdigAlloc`, `MdigControl`, `MdigFree`, `MdigGetHookInfo`, `MdigGrabContinuous`, `MdigHalt`, `MdigInquire`, `MdigProcess`, `MdispAlloc`, `MdispFree`, `MdispSelect`, `MgraText`, `MsysAlloc`, `MsysFree`, `MsysInquire`

**Categories:** Overview, General, Industries, Applications, Modules, Buffer, Display, Graphics, Digitizer, What's New, Older

```cpp
///////////////////////////////////////////////////////////////////////////////
// Aurora Imaging Library
// Filename:    MDigGrabSequence.cpp
//
// Description: This example shows how to record a sequence and play it back in
//              real time.the acquisition can be done in memory only or to
//              an AVI file.
//
// Note:        This example assumes that the hard disk is sufficiently fast
//              to keep up with the grab.Also, removing the sequence display or
//              the text annotation while grabbing will reduce the CPU usageand
//              might help if some frames are missed during acquisition.
//              If the disk or system are not fast enough, choose the option to
//              record uncompressed images to memory.
//
// (C) 1992-2026 Zebra Technologies Corp. and/or its affiliates
// All Rights Reserved
///////////////////////////////////////////////////////////////////////////////

#include <AIOL.h>
#include <stdlib.h>

using namespace Zebra::AuroraImagingObjectLibrary;

class MdigGrabSequenceExample
   {
   private:

      // Sequence file name
      const ailstring _sequenceFile = Paths::Temp + AilText("AilSequence.avi");

      // Quantization factor to use during the compression.
      // Valid values are 1 to 99 (higher to lower quality).
      const int _compressionQFactor = 50;

      // Annotation flag. Set to false to draw the frame number in the saved image.
      static const bool _frameNumberAnnotation = true;

      // Maximum number of images for the multiple buffering grab.
      static const int _nbGrabImageMax = 20;

      static int _numGrab;
      static int _posX;
      static int _posY;

      static const long _stringLengthMax = 20;
      static const long _stringPosX = 20;
      static const long _stringPosY = 20;

      long _nbGrabbedFrames = 0;
      long _nbArchivedFrames = 0;
      bool _saveToDisk = true;

      App::Application _application;
      Sys::System _system;
      Dig::Digitizer _digitizer;
      Buf::Image _imageDisp;
      Buf::CompressedImage _compressedImage;
      Disp::Display _display;
      Gra::Context _graphics;
      std::vector<Buf::Image> _images;
      Buf::Image::SequenceExporter _sequenceExporter;
      Buf::Image::SequenceImporter _sequenceImporter;

   public:
      int64_t RecordFunction(const Dig::ProcessEventArgs& Event)
         {
         try {
            const int STRING_LENGTH_MAX = 20;
            const int STRING_POS_X = 20;
            const int STRING_POS_Y = 20;

            Buf::Image modifiedBuffer = Event.ModifiedBuffer();

            // Increment the frame counter.
            _nbGrabbedFrames++;

            // Print and draw the frame count (remove to reduce CPU usage).
            Os::Printf(AIL_TEXT("Frame #%d\r"), (int)_nbGrabbedFrames);

            AIL_TEXT_CHAR text[STRING_LENGTH_MAX] = AIL_TEXT("0");
            MosSprintf(text, STRING_LENGTH_MAX, AIL_TEXT("%ld"), _nbGrabbedFrames);
            _graphics.DrawString(modifiedBuffer, STRING_POS_X, STRING_POS_Y, (ailstring)text);

            // Copy the new grabbed image to the display.
            Buf::Copy(modifiedBuffer, _imageDisp);

            // Compress the new image if required.
            if (_compressedImage.IsAllocated())
               {
               Buf::Copy(modifiedBuffer, _compressedImage);
               }

            // Record the new image.
            if (_saveToDisk)
               {
               _sequenceExporter.Write(_compressedImage.IsAllocated() ? std::move(_compressedImage) : std::move(modifiedBuffer));
               _nbArchivedFrames++;
               }

            return 0;
            }
         catch(...)
            {
            // User-defined event/processing functions should always handle their own exceptions
            return 1;
            }
         }

      MdigGrabSequenceExample()
         : _application(App::AllocInitFlags::Default)
         , _system(_application)
         , _digitizer(_system)
         , _graphics(_system)
         , _display(_system)
         , _imageDisp(_system, _digitizer.SizeBand(), _digitizer.SizeX(), _digitizer.SizeY(), Buf::AllocType::Unsigned8, Buf::AllocAttributes::ProcDispGrab)
         {

         _compressedImage = Buf::CompressedImage();
         for(int i = 0; i < _nbGrabImageMax; i++)
            {
            _images.emplace_back(Buf::Image());
            }

         _imageDisp.Clear(Color8::Black);
         _display.Select(_imageDisp);
         }

      ~MdigGrabSequenceExample() = default;

      void Run()
         {
         // Grab continuously on display.
         _digitizer.GrabContinuous(_imageDisp);

         // Print a message
         Os::Printf(AilText("\nSEQUENCE ACQUISITION:\n"));
         Os::Printf(AilText("--------------------\n\n"));

         auto licenseModules = _system.OwnerApplication().LicenseModules();

         Os::Printf(AilText("Choose the sequence format:\n"));
         Os::Printf(AilText("1) Uncompressed images to memory (up to %d frames).\n"), _nbGrabImageMax);
         Os::Printf(AilText("2) Uncompressed images to an AVI file.\n"));

         // If sequence is saved to disk, select between grabbing an
         // uncompressed, JPEG or JPEG2000 sequence.
         if((((int)licenseModules & ((int)App::LicenseModules::Jpegstd | (int)App::LicenseModules::Jpeg2000)) != 0))
            {
            if(((int)licenseModules & (int)App::LicenseModules::Jpegstd) != 0)
               {
               Os::Printf(AilText("3) Compressed lossy JPEG images to an AVI file.\n"));
               }
            if(((int)licenseModules & (int)App::LicenseModules::Jpeg2000) != 0)
               {
               Os::Printf(AilText("4) Compressed lossy JPEG2000 images to an AVI file.\n"));
               }
            }

         auto compressAttribute = Buf::AllocAttributes::Null;
         auto validSelection = false;
         while(!validSelection)
            {
            auto selection = Os::Getch();
            validSelection = true;

            // Set the buffer attribute.
            switch(selection)
               {
               case '1':
               case '\r':
                  Os::Printf(AilText("\nUncompressed images to memory selected.\n"));
                  compressAttribute = Buf::AllocAttributes::Null;
                  _saveToDisk = false;
                  break;
               case '2':
                  Os::Printf(AilText("\nUncompressed images to file selected.\n"));
                  compressAttribute = Buf::AllocAttributes::Null;
                  _saveToDisk = true;
                  break;

               case '3':
                  Os::Printf(AilText("\nJPEG images to file selected.\n"));
                  compressAttribute = Buf::AllocAttributes::Compress | Buf::AllocAttributes::JpegLossy;
                  _saveToDisk = true;
                  break;

               case '4':
                  Os::Printf(AilText("\nJPEG 2000 images to file selected.\n"));
                  compressAttribute = Buf::AllocAttributes::Compress | Buf::AllocAttributes::Jpeg2000Lossy;
                  _saveToDisk = true;
                  break;

               default:
                  Os::Printf(AilText("\nInvalid selection !\n"));
                  validSelection = false;
                  break;
               }
            }

         if(compressAttribute != Buf::AllocAttributes::Null)
            {
            _compressedImage.Alloc(_system, _digitizer.SizeBand(), _digitizer.SizeX(), _digitizer.SizeY(), Buf::AllocType::Unsigned8, compressAttribute);
            _compressedImage.QFactor(_compressionQFactor);
            }

         // Allocate the grab buffers to hold the sequence buffering.
         _application.ErrorGlobalMode(Zebra::AuroraImagingObjectLibrary::App::ErrorMode::PrintEnable);

         auto n = 0;
         for(n = 0; n < _nbGrabImageMax; n++)
            {
            if (n == 2)
               _application.ErrorGlobalMode(Zebra::AuroraImagingObjectLibrary::App::ErrorMode::PrintDisable);

            _images[n].Alloc(_system, _digitizer.SizeBand(), _digitizer.SizeX(), _digitizer.SizeY(), Buf::AllocType::Unsigned8, Buf::AllocAttributes::Grab);

            if(_images[n].IsAllocated())
                {
               _images[n].Clear(Color8::White);
                }
            else
               {
                //Pop back unused images
                for (auto f = n; f < _nbGrabImageMax; f++)
                   {
                    _images.pop_back();
                   }
               break;
               }
            }

         _application.ErrorGlobalMode(Zebra::AuroraImagingObjectLibrary::App::ErrorMode::Default);

         // Halt continuous grab.
         _digitizer.Halt();

         // Open the AVI file if required.
         if(_saveToDisk)
            {
            Os::Printf(AilText("\nSaving the sequence to an AVI file...\n"));
            _sequenceExporter.Open(_sequenceFile, false);
            }
         else
            {
            Os::Printf(AilText("\nSaving the sequence to memory...\n\n"));
            }

         // Initialize User's archiving function hook data structure.
         _digitizer.Process.Register(&MdigGrabSequenceExample::RecordFunction, *this);
         if(_saveToDisk)
            {
            _digitizer.Process.Start(_images);
            Os::Printf(AilText("\nPress any key to stop recording.\n\n"));
            Os::Getch();
            }
         else
            {
            _digitizer.Process.Sequence(_images, n);
            }

         // Wait until we have at least 2 frames to avoid an invalid frame rate.
         while(_digitizer.ProcessFrameCount() < 2);

         // Stop the sequence acquisition.
         _digitizer.Process.Stop();
         _digitizer.Process.Unregister();

         // Read and print final statistics.
         auto frameCount = _digitizer.ProcessFrameCount();
         auto frameRate = _digitizer.ProcessFrameRate();
         auto frameMissed = _digitizer.ProcessFrameMissed();

         Os::Printf(AilText("\n\n%d frames recorded (%d missed), at %.1f frames/sec ")
                    AilText("(%.1f ms/frame).\n\n"),
                    (int)_nbGrabbedFrames, frameMissed, frameRate, 1000.0 / frameRate);

         // Sequence file closing if required.
         if(_saveToDisk)
            {
            _sequenceExporter.Close(frameRate);
            }

         // Wait for a key to playback.
         Os::Printf(AilText("Press any key to start the sequence playback.\n"));
         Os::Getch();

         // Playback the sequence until a key is pressed.
         if(_nbGrabbedFrames > 0)
            {
            int64_t keyPressed = 0;
            do
               {
               // If sequence must be loaded.
               if(_saveToDisk)
                  {
                  // Inquire information about the sequence.
                  Os::Printf(AilText("\nPlaying sequence from the AVI file...\n"));
                  Os::Printf(AilText("Press any key to end playback.\n\n"));

                  auto fileInfo = App::FileInformation(_sequenceFile);
                  frameCount = fileInfo.NumberOfImages();
                  frameRate = fileInfo.FrameRate();

                  // Open the sequence file.
                  _sequenceImporter.Open(_sequenceFile);
                  }
               else
                  Os::Printf(AilText("\nPlaying sequence from memory...\n\n"));


               // Copy the images to the screen respecting the sequence frame rate.
               double totalReplay = 0.0;
               int nbFramesReplayed = 0;
               for(n = 0; n < frameCount; n++)
                  {
                  // Reset the time.
                  _application.Timer.Global.Reset();

                  // If image was saved to disk.
                  if(_saveToDisk)
                     {
                     // Load image directly to the display.
                     _sequenceImporter.ReadLoad(_imageDisp, n);
                     _imageDisp.Modified();
                     }
                  else
                     {
                     // Copy the grabbed image to the display.
                     Buf::Copy(_images[n], _imageDisp);
                     }
                  nbFramesReplayed++;
                  Os::Printf(AilText("Frame #%d             \r"), nbFramesReplayed);

                  // Check for a pressed key to exit.
                  if(Os::Kbhit() && (n >= (_nbGrabImageMax - 1)))
                     {
                     Os::Getch();
                     break;
                     }

                  // Wait to have a proper frame rate.
                  auto timeWait = _application.Timer.Global.Read();
                  totalReplay += timeWait;
                  timeWait = (1 / frameRate) - timeWait;
                  _application.Timer.Global.Wait(timeWait);
                  totalReplay += (timeWait > 0) ? timeWait : 0.0;
                  }

               // Close the sequence file.
               if(_saveToDisk)
                  {
                  _sequenceImporter.Close();
                  }

               // Print statistics.
               Os::Printf(AilText("\n\n%d frames replayed, at a frame rate of %.1f frames/sec ")
                          AilText("(%.1f ms/frame).\n\n"),
                          nbFramesReplayed, n / totalReplay, 1000.0 * totalReplay / n);
               Os::Printf(AilText("Press <Enter> to end (or any other key to playback again).\n"));
               keyPressed = Os::Getch();
               } while((keyPressed != '\r') && (keyPressed != '\n'));
            }
         }
   };

int MosMain()
   {
   try
      {
      MdigGrabSequenceExample example;
      example.Run();
      }
   catch(const AIOLException& exception)
      {
      Os::Printf(AilText("Encountered an exception during the example. \n"));
      Os::Printf(AilText("%s \n"), exception.Message().c_str());
      Os::Printf(AilText("Press any key to exit. \n"));
      Os::Getch();
      }
   return 0;
   }

```
