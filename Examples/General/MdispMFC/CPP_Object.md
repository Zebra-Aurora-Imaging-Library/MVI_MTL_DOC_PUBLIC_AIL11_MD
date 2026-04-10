---
title: "MdispMFC"
description: "This example shows how to use AIL in an MFC project. It integrates MFC menus, menu bars, user-created child windows, etc. with AIL displays, buffers, timers and a digitizer."
ms.language: "C++ Object"
---

# MdispMFC

> This example shows how to use AIL in an MFC project. It integrates MFC menus, menu bars, user-created child windows, etc. with AIL displays, buffers, timers and a digitizer.

**Language:** C++ Object

**Functions used:** `MbufClear`, `MappAlloc`, `MappControl`, `MappFree`, `MappHookFunction`, `MbufAllocColor`, `MbufFree`, `MdigAlloc`, `MdigFree`, `MdigInquire`, `MdispAlloc`, `MdispControl`, `MdispFree`, `MdispSelect`, `MsysAlloc`, `MsysFree`, `MsysInquire`

**Categories:** Overview, General, Industries, Applications, Modules, Buffer, Display, Digitizer, What's New, AIL 10.0 SP5, Older

```cpp
///////////////////////////////////////////////////////////////////////////////
// Aurora Imaging Library
// Filename:    MdispMFC.cpp
//
// Description: Defines the class behaviors for the application.
//
// (C) 1992-2026 Zebra Technologies Corp. and/or its affiliates
// All Rights Reserved
///////////////////////////////////////////////////////////////////////////////

#include "stdafx.h"
#include "MdispMFC.h"

#include "MainFrm.h"
#include "ChildFrm.h"
#include "MdispMFCDoc.h"
#include "MdispMFCView.h"

#include "AIOL.h"

#ifdef _DEBUG
#define new DEBUG_NEW
#undef THIS_FILE
static char THIS_FILE[] = __FILE__;
#endif

/////////////////////////////////////////////////////////////////////////////
// CMdispMFCApp

BEGIN_MESSAGE_MAP(CMdispMFCApp, CWinApp)
   //{{AFX_MSG_MAP(CMdispMFCApp)
   ON_COMMAND(ID_APP_ABOUT, OnAppAbout)
   // NOTE - the ClassWizard will add and remove mapping macros here.
   //    DO NOT EDIT what you see in these blocks of generated code!
//}}AFX_MSG_MAP
// Standard file-based document commands
ON_COMMAND(ID_FILE_NEW, CWinApp::OnFileNew)
ON_COMMAND(ID_FILE_OPEN, CWinApp::OnFileOpen)
// Standard print setup command
ON_COMMAND(ID_FILE_PRINT_SETUP, CWinApp::OnFilePrintSetup)
END_MESSAGE_MAP()

/////////////////////////////////////////////////////////////////////////////
// CMdispMFCApp construction

CMdispMFCApp::CMdispMFCApp()
   {
   // TODO: add construction code here
   m_isCurrentlyHookedOnErrors = false;
   // Place all significant initialization in InitInstance
   }

/////////////////////////////////////////////////////////////////////////////
// The one and only CMdispMFCApp object

CMdispMFCApp theApp;

/////////////////////////////////////////////////////////////////////////////
// CMdispMFCApp initialization

BOOL CMdispMFCApp::InitInstance()
   {

   LoadStdProfileSettings();  // Load standard INI file options (including MRU)


   /////////////////////////////////////////////////////////////////////////
   // Aurora Imaging Library: Write your one-time initialization code here 
   /////////////////////////////////////////////////////////////////////////   

   // Allocate an application and a system [CALL TO Aurora Imaging Library]
   m_AilApplication = App::Application(App::AllocInitFlags::Default);
   m_AilSystem = Sys::System(m_AilApplication);

   // Hook AIL error on function DisplayError() [CALL TO Aurora Imaging Library]
   m_AilApplication.ErrorEvent.Hook(&DisplayErrorExt, this);
   m_isCurrentlyHookedOnErrors = true;

   // Disable the typical error message display [CALL TO Aurora Imaging Library]
   m_AilApplication.ErrorGlobalMode(App::ErrorMode::PrintDisable);

   // Inquire the number of digitizers available on the system [CALL TO Aurora Imaging Library]
   m_numberOfDigitizer = m_AilSystem.DigitizerNum();

   // Digitizer is available
   if(m_numberOfDigitizer)
      {
      // Allocate a digitizer [CALL TO Aurora Imaging Library]
      m_AilDigitizer = Dig::Digitizer(m_AilSystem);

      // Inquire digitizer information [CALL TO Aurora Imaging Library]
      m_digitizerSizeX = m_AilDigitizer.SizeX();
      m_digitizerSizeY = m_AilDigitizer.SizeY();
      m_digitizerNbBands = m_AilDigitizer.SizeBand();
      }

   // Initialize the state of the grab
   m_isGrabStarted = FALSE;

   /////////////////////////////////////////////////////////////////////////
   // Aurora Imaging Library: Write your one-time initialization code here 
   /////////////////////////////////////////////////////////////////////////


   // Register the application's document templates.  Document templates
   // serve as the connection between documents, frame windows and views.

   CMultiDocTemplate* pDocTemplate;
   pDocTemplate = new CMultiDocTemplate(
      IDR_MDISPTYPE,
      RUNTIME_CLASS(CMdispMFCDoc),
      RUNTIME_CLASS(CChildFrame), // custom MDI child frame
      RUNTIME_CLASS(CMdispMFCView));
   AddDocTemplate(pDocTemplate);

   // Create main MDI Frame window
   CMainFrame* pMainFrame = new CMainFrame;
   if(!pMainFrame->LoadFrame(IDR_MAINFRAME))
      return FALSE;
   m_pMainWnd = pMainFrame;

   // Parse command line for standard shell commands, DDE, file open
   CCommandLineInfo cmdInfo;
   ParseCommandLine(cmdInfo);

   // Dispatch commands specified on the command line
   if(!ProcessShellCommand(cmdInfo))
      return FALSE;

   // Show and update the initialized main window.
   pMainFrame->ShowWindow(m_nCmdShow);
   pMainFrame->UpdateWindow();

   return TRUE;
   }

/////////////////////////////////////////////////////////////////////////////
// CAboutDlg dialog used for App About

class CAboutDlg: public CDialog
   {
   public:
      CAboutDlg();

      // Dialog Data
         //{{AFX_DATA(CAboutDlg)
      enum { IDD = IDD_ABOUTBOX };
      //}}AFX_DATA

      // ClassWizard generated virtual function overrides
      //{{AFX_VIRTUAL(CAboutDlg)
   protected:
      virtual void DoDataExchange(CDataExchange* pDX);    // DDX/DDV support
      //}}AFX_VIRTUAL

   // Implementation
   protected:
      HICON m_hIcon;
      //{{AFX_MSG(CAboutDlg)
      virtual BOOL OnInitDialog();
      //}}AFX_MSG
      DECLARE_MESSAGE_MAP()
   };

CAboutDlg::CAboutDlg(): CDialog(CAboutDlg::IDD)
   {
   //{{AFX_DATA_INIT(CAboutDlg)
   //}}AFX_DATA_INIT
   m_hIcon = AfxGetApp()->LoadIcon(IDI_IMAGING);

   }

void CAboutDlg::DoDataExchange(CDataExchange* pDX)
   {
   CDialog::DoDataExchange(pDX);
   //{{AFX_DATA_MAP(CAboutDlg)
   //}}AFX_DATA_MAP
   }

BEGIN_MESSAGE_MAP(CAboutDlg, CDialog)
   //{{AFX_MSG_MAP(CAboutDlg)
   //}}AFX_MSG_MAP
END_MESSAGE_MAP()

BOOL CAboutDlg::OnInitDialog()
   {
   CDialog::OnInitDialog();

   SetIcon(m_hIcon, true);       // Set big icon
   SetIcon(m_hIcon, false);     // Set small icon

   return true;  // return TRUE unless you set the focus to a control
   // EXCEPTION: OCX Property Pages should return FALSE
   }

// App command to run the dialog
void CMdispMFCApp::OnAppAbout()
   {
   CAboutDlg aboutDlg;
   aboutDlg.DoModal();
   }

/////////////////////////////////////////////////////////////////////////////
// CMdispMFCApp commands

int CMdispMFCApp::ExitInstance()
   {
   /////////////////////////////////////////////////////////////////////////
   // Aurora Imaging Library: Write your code that will be executed on application exit
   /////////////////////////////////////////////////////////////////////////   

   //Free the digitizer [CALL TO Aurora Imaging Library]
   m_AilDigitizer.Free();

   //Free the system [CALL TO Aurora Imaging Library]
   m_AilSystem.Free();

   if(m_AilApplication.IsAllocated())
      {
      // Enable the typical error message display[CALL TO Aurora Imaging Library]
      m_AilApplication.ErrorGlobalMode(App::ErrorMode::PrintEnable);

      // Unhook error on function DisplayError() [CALL TO Aurora Imaging Library]
      if(m_isCurrentlyHookedOnErrors)
         {
         m_AilApplication.ErrorEvent.UnhookAll();
         m_isCurrentlyHookedOnErrors = false;
         }

      // Free the application [CALL TO Aurora Imaging Library]
      m_AilApplication.Free();
      }

   /////////////////////////////////////////////////////////////////////////
   // Aurora Imaging Library: Write your code that will be executed on application exit
   /////////////////////////////////////////////////////////////////////////

   return CWinApp::ExitInstance();
   }

/////////////////////////////////////////////////////////////////////////
// Aurora Imaging Library: Hook-handler function: DisplayError()
/////////////////////////////////////////////////////////////////////////

int64_t MFTYPE DisplayErrorExt(const AIOLException& exception, void* userDataPtr)
   {
   //If user clicks NO on error message, unhook to errors.
   if(((CMdispMFCApp*)AfxGetApp())->DisplayError(exception, (CWinApp*)userDataPtr) == M_NO)
      {
      ((CMdispMFCApp*)AfxGetApp())->m_AilApplication.ErrorEvent.UnhookAll();
      ((CMdispMFCApp*)AfxGetApp())->HookedOnErrors(false);
      }

   return M_NULL;
   }


long MFTYPE CMdispMFCApp::DisplayError(const AIOLException& exception,
                                       void* userDataPtr)
   {
   CString cErrorMessage = exception.Message().c_str();
   cErrorMessage += "\n\nDo you want to continue error print?";

   return (AfxMessageBox(cErrorMessage, MB_YESNO, 0) == IDYES) ? M_YES : M_NO;
   }

/////////////////////////////////////////////////////////////////////////
// Aurora Imaging Library: Hook-handler function: DisplayError()
/////////////////////////////////////////////////////////////////////////



```
