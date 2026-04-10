---
title: "MdispWindowQt"
description: "This program displays a welcoming message in a user-defined window and grabs (if supported) in it. It uses the AIL system and the MdispSelectWindow() function to display the AIL buffer in a user-created client window."
ms.language: "C++"
---

# MdispWindowQt

> This program displays a welcoming message in a user-defined window and grabs (if supported) in it. It uses the AIL system and the MdispSelectWindow() function to display the AIL buffer in a user-created client window.

**Language:** C++

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

#include <AIL.h>


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
void MessageOutput(QtMsgType type, const QMessageLogContext &context, const QString &msg)
   {
   QByteArray localMsg = msg.toLocal8Bit();
   switch (type)
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


//****************************************************************
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
//***************************************************************
void AilApplication(PaintArea *Area)
   {
   // Variables 
   AIL_ID AilApplication, // Application identifier.
      AilDisplay,
      AilSystem,      // System identifier.       
      AilDigitizer,   // Digitizer identifier.    
      AilImage;       // Image buffer identifier. 

   AIL_INT BufSizeX    = 0;
   AIL_INT BufSizeY    = 0;
   AIL_INT BufSizeBand = 0;

   // Allocate an application. 
   MappAlloc(M_NULL, M_DEFAULT, &AilApplication);

   // Allocate a system. 
   MsysAlloc(M_DEFAULT, AIL_TEXT("M_DEFAULT"), M_DEFAULT, M_DEFAULT, &AilSystem);

   // Allocate a display. 
   MdispAlloc(AilSystem, M_DEFAULT, AIL_TEXT("M_DEFAULT"), M_WINDOWED, &AilDisplay);

   // Allocate a digitizer if supported and sets the target image size. 
   if (MsysInquire(AilSystem, M_DIGITIZER_NUM, M_NULL) > 0)
      {
      MdigAlloc(AilSystem, M_DEFAULT, AIL_TEXT("M_DEFAULT"), M_DEFAULT, &AilDigitizer);
      MdigInquire(AilDigitizer, M_SIZE_X,    &BufSizeX);
      MdigInquire(AilDigitizer, M_SIZE_Y,    &BufSizeY);
      MdigInquire(AilDigitizer, M_SIZE_BAND, &BufSizeBand);
      // Resize the display window 
      if((BufSizeX > DEFAULT_IMAGE_SIZE_X) || (BufSizeY > DEFAULT_IMAGE_SIZE_Y))
         {
         if(Area->parentWidget())
            {
            AilWindow* MainWindow = (AilWindow*)Area->parentWidget();
            MainWindow->resize(BufSizeX, BufSizeY+ MainWindow->ToolBar()->height());
            }
         }
      }
   else
      {
      AilDigitizer = M_NULL;
      BufSizeX     = DEFAULT_IMAGE_SIZE_X;
      BufSizeY     = DEFAULT_IMAGE_SIZE_Y;
      BufSizeBand  = DEFAULT_IMAGE_SIZE_BAND;
      }

   // Allocate a buffer. 
   MbufAllocColor(AilSystem, BufSizeBand, BufSizeX, BufSizeY, 8+M_UNSIGNED,
                  (AilDigitizer ? M_IMAGE+M_DISP+M_GRAB : M_IMAGE+M_DISP), &AilImage);

   // Clear the buffer 
   MbufClear(AilImage,0);

   // Select the buffer to be displayed in the user-specified window 
   MdispSelectWindow(AilDisplay, AilImage, (AIL_WINDOW_HANDLE)Area->UserWindowHandle());

   // Print a string in the image buffer using AIL.
   // Note: When a buffer is modified using a command, the display
   // automatically updates the window passed to MdispSelectWindow().
   MgraFont(M_DEFAULT, M_FONT_DEFAULT_MEDIUM);
   MgraText(M_DEFAULT, AilImage, (BufSizeX/8)*2 + 20, BufSizeY/2,
            AIL_TEXT("Aurora Imaging Library"));
   MgraRect(M_DEFAULT, AilImage, ((BufSizeX/8)*2)-60, (BufSizeY/2)-80,
            ((BufSizeX/8)*2)+370, (BufSizeY/2)+100);
   MgraRect(M_DEFAULT, AilImage, ((BufSizeX/8)*2)-40, (BufSizeY/2)-60,
            ((BufSizeX/8)*2)+350, (BufSizeY/2)+80);
   MgraRect(M_DEFAULT, AilImage, ((BufSizeX/8)*2)-20, (BufSizeY/2)-40,
            ((BufSizeX/8)*2)+330, (BufSizeY/2)+60);

   // Open a message box to wait for a key. 
   QMessageBox::information(0, "application example",
                            "\"Aurora Imaging Library\" was printed", QMessageBox::Ok);

   // Grab in the user window if supported. 
   if (AilDigitizer)
      {
      // Grab continuously. 
      MdigGrabContinuous(AilDigitizer, AilImage);

      // Open a message box to wait for a key. 
      QMessageBox::information(0, "application example",
                               "Continuous grab in progress", QMessageBox::Ok);

      // Stop continuous grab. 
      MdigHalt(AilDigitizer);
      }
   Area->setAttribute(Qt::WA_OpaquePaintEvent, false);
   Area->setAttribute(Qt::WA_PaintOnScreen, false);

   // Remove the buffer from the display. 
   MdispSelect(AilDisplay, M_NULL);

   // Free allocated objects. 
   MbufFree(AilImage);


   if (AilDigitizer)
      MdigFree(AilDigitizer);
   MdispFree(AilDisplay);
   MsysFree(AilSystem);
   MappFree(AilApplication);
   }


PaintArea::PaintArea(QWidget* parent)
   : QWidget(parent)
   , m_UserWindowHandle(0)
   {
   }

void PaintArea::startAil()
   {
   setAttribute(Qt::WA_OpaquePaintEvent);
   setAttribute(Qt::WA_PaintOnScreen);
   setAttribute(Qt::WA_NoSystemBackground);
   m_UserWindowHandle = winId();
   AilApplication(this);
   repaint();
   }

bool PaintArea::event(QEvent* e )
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

   QAction *startAct = new QAction(tr("&Start"), this);
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


//****************************************************************
//
//   Name:     main()
//
//   Synopsis: Call initialization function, processes message loop.
//
//***************************************************************
int main(int argc, char* argv[])
   {
#if M_AIL_USE_LINUX
   qputenv("QT_QPA_PLATFORM", "xcb");
   XInitThreads();
#if QT_VERSION >= QT_VERSION_CHECK(5, 0, 0)   
   qInstallMessageHandler(MessageOutput);
#endif
#endif
   QApplication a(argc, argv);
   AilWindow *w = new AilWindow;
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
