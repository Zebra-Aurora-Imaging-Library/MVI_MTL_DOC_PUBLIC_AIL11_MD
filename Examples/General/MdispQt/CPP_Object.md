---
title: "MdispQt"
description: "This example shows how to use AIL in an Qt project. It integrates Qt menus, menu bars, user-created child windows, etc. with AIL displays, buffers, timers and a digitizer."
ms.language: "C++ Object"
---

# MdispQt

> This example shows how to use AIL in an Qt project. It integrates Qt menus, menu bars, user-created child windows, etc. with AIL displays, buffers, timers and a digitizer.

**Language:** C++ Object

**Functions used:** `MbufClear`, `MappAlloc`, `MappControl`, `MappFree`, `MappHookFunction`, `MbufAllocColor`, `MbufFree`, `MdigAlloc`, `MdigFree`, `MdigInquire`, `MdispAlloc`, `MdispControl`, `MdispFree`, `MdispSelect`, `MsysAlloc`, `MsysFree`, `MsysInquire`

**Categories:** Overview, General, Industries, Applications, Modules, Buffer, Display, Digitizer, What's New, AIL 10.0 SP5, Older

```cpp
﻿///////////////////////////////////////////////////////////////////////////////
// Aurora Imaging Library
// Filename:    MdispQT.cpp
//
// (C) 1992-2026 Zebra Technologies Corp. and/or its affiliates
// All Rights Reserved
///////////////////////////////////////////////////////////////////////////////

#ifdef _MSC_VER
#pragma warning(disable:4127)
#endif
#include <QtGui>
#if QT_VERSION >= QT_VERSION_CHECK(5, 0, 0)
#include <QApplication>
#endif
#include "mainframe.h"
#include "mdispqtapp.h"

#if STATIC_QT
#include <QtPlugin>
#ifdef WIN32
Q_IMPORT_PLUGIN(QWindowsIntegrationPlugin)
#else
Q_IMPORT_PLUGIN(QXcbIntegrationPlugin)
Q_IMPORT_PLUGIN(QXcbGlxIntegrationPlugin)
#endif
#endif
#ifdef _MSC_VER
#pragma warning(default:4127)
#endif

#if M_AIL_USE_LINUX
#include <X11/Xlib.h>
#undef Bool
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

int main(int argc, char* argv[])
   {
#if M_AIL_USE_LINUX
   qputenv("QT_QPA_PLATFORM", "xcb");
   // implicit pointer grab doesn't work under Wayland or when using Qt 6.x.
   qputenv("QT_XCB_NO_XI2", "1");
   XInitThreads();
#if QT_VERSION >= QT_VERSION_CHECK(5, 0, 0)   
   qInstallMessageHandler(MessageOutput);
#endif
#else
   qputenv("QT_QPA_PLATFORM", "windows:nowmpointer");
#endif

   MdispQtApp app(argc, argv);
   return app.exec();

   }

```
