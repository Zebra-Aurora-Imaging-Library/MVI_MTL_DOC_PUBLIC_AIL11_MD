---
title: "MdigGrabSequence"
description: "This example shows how to grab a sequence and archive it in real-time."
ms.language: "C# Object"
---

# MdigGrabSequence

> This example shows how to grab a sequence and archive it in real-time.

**Language:** C# Object

**Functions used:** `MbufImportSequence`, `MappAlloc`, `MappControl`, `MappFree`, `MappInquire`, `MappTimer`, `MbufAllocColor`, `MbufClear`, `MbufCopy`, `MbufDiskInquire`, `MbufExportSequence`, `MbufFree`, `MbufControl`, `MdigAlloc`, `MdigControl`, `MdigFree`, `MdigGetHookInfo`, `MdigGrabContinuous`, `MdigHalt`, `MdigInquire`, `MdigProcess`, `MdispAlloc`, `MdispFree`, `MdispSelect`, `MgraText`, `MsysAlloc`, `MsysFree`, `MsysInquire`

**Categories:** Overview, General, Industries, Applications, Modules, Buffer, Display, Graphics, Digitizer, What's New, Older

```csharp
﻿///////////////////////////////////////////////////////////////////////////////
// Aurora Imaging Library
// Filename:    MdigGrabSequence.cs
//
// Description: This example shows how to grab a sequence, archive it, and play 
//              it back in real time from an AVI file.
//
// Note:        This example assumes that the hard disk is sufficiently fast 
//              to keep up with the grab. Also, removing the sequence display or 
//              the text annotation while grabbing will reduce the CPU usage and
//              might help if some frames are missed during acquisition. 
//              If the disk or system are not fast enough, set
//              SAVE_SEQUENCE_TO_DISK to false.
//
// (C) 1992-2026 Zebra Technologies Corp. and/or its affiliates 
// All Rights Reserved
///////////////////////////////////////////////////////////////////////////////

using System;
using System.Collections.Generic;
using System.IO;
using System.Runtime.InteropServices;
using Zebra.AuroraImagingObjectLibrary;

namespace MdigGrabSequence
    {
    class MdigGrabSequenceExample : IDisposable
        {
        void RecordFunction(object sender, Dig.ProcessEventArgs e)
            {
            try
                {
                const int StringPosX = 20;
                const int StringPosY = 20;

                Buf.Image modifiedBuffer = e.ModifiedBuffer;

                // Increment the frame counter.
                _nbGrabbedFrames++;

                // Print and draw the frame count (remove to reduce CPU usage).
                Console.Write($"Frame #{(int)_nbGrabbedFrames}\r");

                _graContext.DrawString(modifiedBuffer, StringPosX, StringPosY, _nbGrabbedFrames.ToString());

                if (_compressedImage.IsAllocated)
                    {
                    Buf.Copy(modifiedBuffer, _compressedImage);
                    }

                if (_saveToDisk)
                    {
                    Buf.Image imageToExport;
                    if (_compressedImage.IsAllocated)
                        {
                        imageToExport = _compressedImage;
                        }
                    else
                        {
                        imageToExport = modifiedBuffer;
                        }
                    _sequenceExporter.Write(imageToExport);
                    _nbArchivedFrames++;
                    }

                // Copy the new grabbed image to the display.
                Buf.Copy(modifiedBuffer, _imageDisp);
                }
            catch
                {
                // User-defined event/processing functions should always handle their own exceptions
                }
            }

        public MdigGrabSequenceExample()
            {
            _application = new App.Application(App.AllocInitFlags.Default);
            _system = new Sys.System(_application);
            _digitizer = new Dig.Digitizer(_system);
            _graContext = new Gra.Context(_system);
            _display = new Disp.Display(_system);
            _imageDisp = new Buf.Image(_system, 3, _digitizer.SizeX, _digitizer.SizeY, Buf.AllocType.Unsigned8, Buf.AllocAttributes.ProcDispGrab);

            _compressedImage = new Buf.CompressedImage();

            _images = new List<Buf.Image>();

            _imageDisp.Clear(Color8.Black);
            _display.Select(_imageDisp);
            }

        public void Run()
            {
            var compressAttribute = Buf.AllocAttributes.Null;
            int n = 0;

            // Grab continuously on display.
            _digitizer.GrabContinuous(_imageDisp);

            // Print a message
            Console.WriteLine("");
            Console.WriteLine("SEQUENCE ACQUISITION:");
            Console.WriteLine("--------------------");
            Console.WriteLine("");

            var licenseModules = _application.LicenseModules;

            Console.WriteLine("Choose the sequence format:");
            Console.WriteLine($"1) Uncompressed images to memory (up to {_nbGrabImageMax} frames).");
            Console.WriteLine("2) Uncompressed images to an AVI file.");

            // If sequence is saved to disk, select between grabbing an 
            // uncompressed, JPEG or JPEG2000 sequence. 
            if ((((int)licenseModules & ((int)App.LicenseModules.Jpegstd | (int)App.LicenseModules.Jpeg2000)) != 0))
                {
                if (((int)licenseModules & (int)App.LicenseModules.Jpegstd) != 0)
                    {
                    Console.WriteLine("3) Compressed lossy JPEG images to an AVI file.");
                    }
                if (((int)licenseModules & (int)App.LicenseModules.Jpeg2000) != 0)
                    {
                    Console.WriteLine("4) Compressed lossy JPEG2000 images to an AVI file.");
                    }
                }

            bool validSelection = false;
            while (!validSelection)
                {
                var selection = Console.ReadKey(true);
                validSelection = true;

                // Set the buffer attribute.
                switch (selection.Key)
                    {
                    case ConsoleKey.NumPad1:
                    case ConsoleKey.D1:
                    case ConsoleKey.Enter:
                        Console.WriteLine("");
                        Console.WriteLine("Uncompressed images to memory selected.");
                        Console.WriteLine("");
                        compressAttribute = Buf.AllocAttributes.Null;
                        _saveToDisk = false;
                        break;
                    case ConsoleKey.NumPad2:
                    case ConsoleKey.D2:
                        Console.WriteLine("");
                        Console.WriteLine("Uncompressed images to file selected.");
                        Console.WriteLine("");
                        compressAttribute = Buf.AllocAttributes.Null;
                        _saveToDisk = true;
                        break;
                    case ConsoleKey.NumPad3:
                    case ConsoleKey.D3:
                        Console.WriteLine("");
                        Console.WriteLine("JPEG images to file selected.");
                        Console.WriteLine("");
                        compressAttribute = Buf.AllocAttributes.Compress | Buf.AllocAttributes.JpegLossy;
                        _saveToDisk = true;
                        break;
                    case ConsoleKey.NumPad4:
                    case ConsoleKey.D4:
                        Console.WriteLine("");
                        Console.WriteLine("JPEG 2000 images to file selected.");
                        Console.WriteLine("");
                        compressAttribute = Buf.AllocAttributes.Compress | Buf.AllocAttributes.Jpeg2000Lossy;
                        _saveToDisk = true;
                        break;
                    default:
                        Console.WriteLine("");
                        Console.WriteLine("Invalid selection !");
                        Console.WriteLine("");
                        validSelection = false;
                        break;
                    }
                }

            // Allocate a compressed buffer if required.
            if (compressAttribute != Buf.AllocAttributes.Null)
                {
                _compressedImage.Alloc(_system, _digitizer.SizeBand, _digitizer.SizeX, _digitizer.SizeY, Buf.AllocType.Unsigned8, compressAttribute);
                _compressedImage.QFactor = _compressionQFactor;
                }

            // Allocate the grab buffers to hold the sequence buffering.
            _application.ErrorGlobalMode = App.ErrorMode.PrintEnable;
            for (n = 0; n < _nbGrabImageMax; n++)
                {
                if (n == 2)
                    {
                    _application.ErrorGlobalMode = App.ErrorMode.PrintDisable;
                    }

                var image = new Buf.Image(_system, _digitizer.SizeBand, _digitizer.SizeX, _digitizer.SizeY, Buf.AllocType.Unsigned8, Buf.AllocAttributes.Grab);

                if (image.IsAllocated)
                    {
                    image.Clear(Color8.White);
                    _images.Add(image);
                    }
                else
                    {
                    break;
                    }
                }

            _application.ErrorGlobalMode = App.ErrorMode.Default;
            
            // Halt continuous grab.
            _digitizer.Halt();

            // Open the AVI file if required.
            if (_saveToDisk)
                {
                Console.WriteLine("Saving the sequence to an AVI file...");
                Console.WriteLine("");
                _sequenceExporter = new Buf.Image.SequenceExporter(_sequenceFile, false);
                }
            else
                {
                Console.WriteLine("Saving the sequence to memory...");
                Console.WriteLine("");
                }

            _digitizer.Process.OnDigProcessEvent += RecordFunction;

            if (_saveToDisk)
                {
                _digitizer.Process.Start(_images);
                Console.WriteLine("Press any key to stop recording.");
                Console.WriteLine();
                Console.ReadKey(true);
                }
            else
                {
                _digitizer.Process.Sequence(_images);
                }

            // Wait until we have at least 2 frames to avoid an invalid frame rate.
            while (_digitizer.ProcessFrameCount < 2)
                { };

            // Stop the sequence acquisition.
            _digitizer.Process.Stop();
            _digitizer.Process.OnDigProcessEvent -= RecordFunction;

            // Read and print final statistics.
            var frameCount = _digitizer.ProcessFrameCount;
            var frameRate = _digitizer.ProcessFrameRate;
            var frameMissed = _digitizer.ProcessFrameMissed;

            Console.WriteLine();
            Console.WriteLine();
            Console.WriteLine("{0} frames recorded ({1} missed), at {2:0.0} frames/sec ({3:0.0} ms/frame).", _nbGrabbedFrames, frameMissed, frameRate, 1000.0 / frameRate);
            Console.WriteLine();

            // Sequence file closing if required.
            if (_saveToDisk)
                {
                _sequenceExporter.Close((long)frameRate);
                }

            // Wait for a key to playback.
            Console.WriteLine("Press any key to start the sequence playback.\n");
            Console.ReadKey(true);

            // Playback the sequence until a key is pressed.
            if (_nbGrabbedFrames > 0)
                {
                do
                    {
                    // If sequence must be loaded.
                    if (_saveToDisk)
                        {
                        // Inquire information about the sequence.
                        Console.WriteLine("Playing sequence from the AVI file...");
                        Console.WriteLine("Press any key to end playback.\n");

                        var fileInfo = new App.Application.FileInformation(_sequenceFile);
                        frameCount = fileInfo.NumberOfImages;
                        frameRate = fileInfo.FrameRate;
                        var compressionType = fileInfo.CompressionType;

                        // Open the sequence file.
                        _sequenceImporter = new Buf.Image.SequenceImporter(_sequenceFile);
                        }
                    else
                        {
                        Console.WriteLine("\nPlaying sequence from memory...\n\n");
                        }

                    // Copy the images to the screen respecting the sequence frame rate.
                    var totalReplay = 0.0;
                    var nbFramesReplayed = 0;
                    for (n = 0; n < frameCount; n++)
                        {
                        // Reset the time.
                        _application.Timer.Global.Reset();

                        // If image was saved to disk.
                        if (_saveToDisk)
                            {
                            // Load image directly to the display.
                            _sequenceImporter.ReadLoad(_imageDisp, n);
                            nbFramesReplayed++;
                            Console.Write("Frame #{0}             \r", nbFramesReplayed);
                            }
                        else
                            {
                            // Copy the grabbed image to the display.
                            Buf.Copy(_images[n], _imageDisp);
                            nbFramesReplayed++;
                            Console.Write("Frame #{0}             \r", nbFramesReplayed);
                            }

                        // Check for a pressed key to exit.
                        if (Console.KeyAvailable && (n >= (_nbGrabImageMax - 1)))
                            {
                            Console.ReadKey(true);
                            break;
                            }

                        // Wait to have a proper frame rate.
                        var timeWait = _application.Timer.Global.Read();
                        totalReplay += timeWait;
                        timeWait = (1 / frameRate) - timeWait;
                        _application.Timer.Global.Wait(timeWait);
                        totalReplay += (timeWait > 0) ? timeWait : 0.0;
                        }

                    // Close the sequence file.
                    if (_saveToDisk)
                        {
                        _sequenceImporter.Close();
                        }

                    // Print statistics.
                    Console.WriteLine();
                    Console.WriteLine();
                    Console.WriteLine("{0} frames replayed, at a frame rate of {1:0.0} frames/sec ({2:0.0} ms/frame).", nbFramesReplayed, n / totalReplay, 1000.0 * totalReplay / n);
                    Console.WriteLine();
                    Console.WriteLine("Press <Enter> to end (or any other key to playback again).");
                    } while (Console.ReadKey(true).Key != ConsoleKey.Enter);
                }
            }

        public void Dispose()
            {
            _display?.Free();
            _graContext?.Free();
            _imageDisp?.Free();
            _images?.ForEach(im => im.Free());
            _compressedImage?.Free();
            _digitizer?.Free();
            _system?.Free();
            _application?.Free();
            }

        // Sequence file name
        private string _sequenceFile = Path.Combine(Paths.Temp, "AilSequence.avi");

        // Quantization factor to use during the compression.
        // Valid values are 1 to 99 (higher to lower quality).
        private const int _compressionQFactor = 50;

        // Maximum number of images for the multiple buffering grab.
        private const int _nbGrabImageMax = 20;

        private readonly App.Application _application;
        private readonly Sys.System _system;
        private readonly Dig.Digitizer _digitizer;
        private readonly Disp.Display _display;
        private readonly Buf.Image _imageDisp;
        private readonly Buf.CompressedImage _compressedImage;
        private readonly Gra.Context _graContext;
        private List<Buf.Image> _images;

        private long _nbGrabbedFrames = 0;
        private long _nbArchivedFrames = 0;
        private bool _saveToDisk = false;

        Buf.Image.SequenceExporter _sequenceExporter;
        Buf.Image.SequenceImporter _sequenceImporter;
        }

    class Program
        {
        static void Main(string[] args)
            {
            try
                {
                using (var example = new MdigGrabSequenceExample())
                    {
                    example.Run();
                    }
                }
            catch (AIOLException exception)
                {
                Console.WriteLine("Encountered an exception during the example.");
                Console.WriteLine(exception.Message);
                Console.WriteLine("Press any key to exit.");
                Console.ReadKey();
                }
            }
        }
    }

```
