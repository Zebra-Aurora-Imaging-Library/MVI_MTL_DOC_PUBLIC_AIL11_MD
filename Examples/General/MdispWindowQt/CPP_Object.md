---
title: "MdispWindowQt"
description: "This program displays a welcoming message in a user-defined window and grabs (if supported) in it. It uses the AIL system and the MdispSelectWindow() function to display the AIL buffer in a user-created client window."
ms.language: "C++ Object"
---

# MdispWindowQt

> This program displays a welcoming message in a user-defined window and grabs (if supported) in it. It uses the AIL system and the MdispSelectWindow() function to display the AIL buffer in a user-created client window.

**Language:** C++ Object

**Functions used:** `MdispFree`, `MappAlloc`, `MappFree`, `MbufAllocColor`, `MbufClear`, `MbufFree`, `MdigAlloc`, `MdigFree`, `MdigGrabContinuous`, `MdigHalt`, `MdigInquire`, `MdispAlloc`, `MdispSelect`, `MdispSelectWindow`, `MgraFont`, `MgraRect`, `MgraText`, `MsysAlloc`, `MsysFree`, `MsysInquire`

**Categories:** Overview, General, Industries, Applications, Modules, Buffer, Display, Graphics, Digitizer, What's New, Older

```cpp
///////////////////////////////////////////////////////////////////////////////
// Aurora Imaging Library
// Filename:    MDispWindowQt.cpp
//
// Description: This program displays a welcoming message in a user-defined
//              window and grabs (if supported) in it. It uses
//              the system and the MdispSelectWindow() function
//              to display the buffer in a user-created client window.
//         
//              This version uses the Qt library to create the client window.
//
// (C) 1992-2026 Zebra Technologies Corp. and/or its affiliates
// All Rights Reserved
///////////////////////////////////////////////////////////////////////////////

#include "MdispWindowQt.h"
#ifdef _MSC_VER
#pragma warning(disable:4127)
#endif
#include <QApplication>
#include <QAction>
#include <QMainWindow>
#include <QMessageBox>
#include <QToolBar>
#ifdef _MSC_VER
#pragma warning(default:4127)
#endif

#include <AIOL.h>
using namespace Zebra::AuroraImagingObjectLibrary;

#if STATIC_QT
#include <QtPlugin>
#if !M_AIL_USE_LINUX
Q_IMPORT_PLUGIN(QWindowsIntegrationPlugin)
#else
Q_IMPORT_PLUGIN(QXcbIntegrationPlugin)
#endif
#endif

#if M_AIL_USE_LINUX
#include <X11/Xlib.h>
#if QT_VERSION >= QT_VERSION_CHECK(5, 0, 0)
void MessageOutput(QtMsgType type, const QMessageLogContext& context, const QString& msg)
   {
   QByteArray localMsg = msg.toLocal8Bit();
   switch(type)
      {
      case QtDebugMsg:
         fprintf(stderr, "Debug: %s (%s:%u, %s)\n", localMsg.constData(), context.file, context.line, context.function);
         break;
      case QtWarningMsg:
#if QT_VERSION >= QT_VERSION_CHECK(5, 5, 0)         
      case QtInfoMsg:
#endif
         break;
      case QtCriticalMsg:
         fprintf(stderr, "Critical: %s (%s:%u, %s)\n", localMsg.constData(), context.file, context.line, context.function);
         break;
      case QtFatalMsg:
         fprintf(stderr, "Fatal: %s (%s:%u, %s)\n", localMsg.constData(), context.file, context.line, context.function);
         abort();
      }
   }
#endif
#endif


// Window title.
#define AIL_APPLICATION_NAME      "Aurora Imaging Library application"

// Default image dimensions.
#define DEFAULT_IMAGE_SIZE_X       640
#define DEFAULT_IMAGE_SIZE_Y       480
#define DEFAULT_IMAGE_SIZE_BAND      1

 //////////////////////////////////////////////////////////////////////////
 //
 // Name:         AilApplication()
 //
 // synopsis:     This function is the core of the application that
 //               is executed when this program is started. See main()
 //               below for the program entry point.
 //
 //               It uses Aurora Imaging Library to display a welcoming message in the
 //               specified user window and to grab in it if it is
 //               supported by the target system.
 //
 //////////////////////////////////////////////////////////////////////////

void AilApplication(PaintArea* Area)
   {
   // Allocate an application.
   auto ailApplication = App::Application(App::AllocInitFlags::Default);

   // Allocate a system.
   auto ailSystem = Sys::System(ailApplication);

   // Allocate a display.
   auto ailDisplay = Disp::Display(ailSystem, AilText("M_DEFAULT"), Disp::AllocDispNums::Default, Disp::AllocInitFlags::Windowed);

   // Allocate a digitizer if supported and sets the target image size.
   Dig::Digitizer ailDigitizer;
   int64_t bufSizeX, bufSizeY, bufSizeBand;
   if(ailSystem.DigitizerNum() > 0)
      {
      ailDigitizer = Dig::Digitizer(ailSystem);
      bufSizeX = ailDigitizer.SizeX();
      bufSizeY = ailDigitizer.SizeY();
      bufSizeBand = ailDigitizer.SizeBand();

      // Resize the display window
      if((bufSizeX > DEFAULT_IMAGE_SIZE_X) || (bufSizeY > DEFAULT_IMAGE_SIZE_Y))
         {
         if(Area->parentWidget())
            {
            AilWindow* MainWindow = (AilWindow*)Area->parentWidget();
            MainWindow->resize(bufSizeX, bufSizeY + MainWindow->ToolBar()->height());
            }
         }
      }
   else
      {
      bufSizeX = DEFAULT_IMAGE_SIZE_X;
      bufSizeY = DEFAULT_IMAGE_SIZE_Y;
      bufSizeBand = DEFAULT_IMAGE_SIZE_BAND;
      }

   // Allocate a buffer.
   auto ailImage = Buf::Image(ailSystem, bufSizeBand, bufSizeX, bufSizeY, Buf::AllocType::Unsigned8, (ailDigitizer.IsAllocated() ? Buf::AllocAttributes::Disp | Buf::AllocAttributes::Grab : Buf::AllocAttributes::Disp));

   // Clear the buffer
   ailImage.Clear(Color8::Black);

   // Select the buffer to be displayed in the user-specified window
   ailDisplay.SelectWindow(ailImage, (AIL_WINDOW_HANDLE)Area->UserWindowHandle());

   // Print a string in the image buffer using AIL.
   //    Note: When a buffer is modified using a command, the display
   //    automatically updates the window passed to MdispSelectWindow().
   auto ailGraphicContext = Gra::Context(ailSystem);
   ailGraphicContext.SetFont(Gra::FontName::DefaultMedium);
   ailGraphicContext.DrawString(ailImage, ((bufSizeX / 8) * 2) + 20, bufSizeY / 2, AilText("Aurora Imaging Library"));
   ailGraphicContext.DrawRect(ailImage, ((bufSizeX / 8) * 2) - 60, (bufSizeY / 2) - 80, ((bufSizeX / 8) * 2) + 370, (bufSizeY / 2) + 100);
   ailGraphicContext.DrawRect(ailImage, ((bufSizeX / 8) * 2) - 40, (bufSizeY / 2) - 60, ((bufSizeX / 8) * 2) + 350, (bufSizeY / 2) + 80);
   ailGraphicContext.DrawRect(ailImage, ((bufSizeX / 8) * 2) - 20, (bufSizeY / 2) - 40, ((bufSizeX / 8) * 2) + 330, (bufSizeY / 2) + 60);

   // Open a message box to wait for a key.
   QMessageBox::information(0, "application example", "\"Aurora Imaging Library\" was printed", QMessageBox::Ok);

   // Grab in the user window if supported.
   if(ailDigitizer.IsAllocated())
      {
      // Grab continuously.
      ailDigitizer.GrabContinuous(ailImage);

      // Open a message box to wait for a key.
      QMessageBox::information(0, "application example", "Continuous grab in progress", QMessageBox::Ok);

      // Stop continuous grab.
      ailDigitizer.Halt();
      }
   Area->setAttribute(Qt::WA_OpaquePaintEvent, false);
   Area->setAttribute(Qt::WA_PaintOnScreen, false);

   // Remove the buffer from the display.
   ailDisplay.Deselect();

   // Free allocated objects.
   ailImage.Free();

   if(ailDigitizer.IsAllocated())
      ailDigitizer.Free();
   ailGraphicContext.Free();
   ailDisplay.Free();
   ailSystem.Free();
   ailApplication.Free();
   }

PaintArea::PaintArea(QWidget* parent)
   : QWidget(parent)
   , m_UserWindowHandle(0)
   {}

void PaintArea::startAil()
   {
   setAttribute(Qt::WA_OpaquePaintEvent);
   setAttribute(Qt::WA_PaintOnScreen);
   setAttribute(Qt::WA_NoSystemBackground);
   m_UserWindowHandle = winId();
   AilApplication(this);
   repaint();
   }

bool PaintArea::event(QEvent* e)
   {

#if QT_VERSION >= 0x040602 && M_AIL_USE_LINUX
   if(e->type() == QEvent::WinIdChange || e->type() == QEvent::Show)
#else
   if(e->type() == QEvent::Show)
#endif
      m_UserWindowHandle = winId();
   return QWidget::event(e);
   }

QSize PaintArea::sizeHint() const
   {
   return QSize(DEFAULT_IMAGE_SIZE_X, DEFAULT_IMAGE_SIZE_Y);
   }

AilWindow::AilWindow()
   : QMainWindow(),
   m_PaintArea(0)
   {

   setWindowTitle(AIL_APPLICATION_NAME);

   QAction* startAct = new QAction(tr("&Start"), this);
   startAct->setShortcut(tr("Ctrl+s"));
   connect(startAct, &QAction::triggered, this, &AilWindow::start);

   m_Tools = new QToolBar(tr("Tool Bar"), this);
   m_Tools->setMovable(false);
   m_Tools->addAction(startAct);
   addToolBar(m_Tools);

   m_PaintArea = new PaintArea(this);
   m_PaintArea->resize(DEFAULT_IMAGE_SIZE_X, DEFAULT_IMAGE_SIZE_Y);
   setCentralWidget(m_PaintArea);

   }

void AilWindow::start()
   {
   m_PaintArea->startAil();
   }


///////////////////////////////////////////////////////////////////////
//
//   Name:     main()
//
//   Synopsis: Call initialization function, processes message loop.
//
///////////////////////////////////////////////////////////////////////

int main(int argc, char* argv[])
   {
#if M_AIL_USE_LINUX
   XInitThreads();
#if QT_VERSION >= QT_VERSION_CHECK(5, 0, 0)   
   qInstallMessageHandler(MessageOutput);
#endif
#endif
   QApplication a(argc, argv);
   AilWindow* w = new AilWindow;
   w->show();
   return a.exec();
   }

#if  QT_VERSION < QT_VERSION_CHECK(6, 0, 0)
#if (M_AIL_USE_LINUX && !STATIC_QT) || (M_AIL_USE_WINDOWS)
#include "moc_MdispWindowQt.cpp"
#endif
#else
#if M_AIL_USE_WINDOWS || !STATIC_QT
#include "moc_MdispWindowQt.cpp"
#endif
#endif

```
