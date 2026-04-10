---
title: "MedgeDispTk"
description: "This example shows how to use AIL in an Python project using Tk. It integrates Tk frames, buttons, user-created child windows, etc. with AIL displays, buffers, and processing. Note that this example requires the Python TkInter GUI package to be installed."
ms.language: "Python"
---

# MedgeDispTk

> This example shows how to use AIL in an Python project using Tk. It integrates Tk frames, buttons, user-created child windows, etc. with AIL displays, buffers, and processing. Note that this example requires the Python TkInter GUI package to be installed.

**Language:** Python

**Functions used:** `MappAlloc`, `MappFree`, `MbufFree`, `MbufInquire`, `MbufRestore`, `MdispAlloc`, `MdispControl`, `MdispFree`, `MdispSelectWindow`, `MedgeAlloc`, `MedgeAllocResult`, `MedgeCalculate`, `MedgeDraw`, `MedgeFree`, `MedgeGetResult`, `MgraAllocList`, `MgraFree`, `MsysAlloc`, `MsysFree`

**Categories:** Overview, General, Industries, Applications, Modules, Buffer, Display, Edge Finder, Graphics, What's New, Older

```python
#!/usr/bin/env python3
# -*- coding: utf-8 -*-
##########################################################################
#
# 
#  File name: MedgeDispTk.py  
#
#   Synopsis:  This program shows how to embed a display 
#              into a Tkinter frame using Python language.
#
#  (C) 1992-2026 Zebra Technologies Corp. and/or its affiliates
#  All Rights Reserved
##########################################################################

import sys
import os

try:
   import ail11 as AIL
except:
   print("Import Aurora Imaging Library failure.")
   print("An error occurred while trying to import ail11. Please make sure ail is under your python path.\n")
   print("Press any key to end.\n")
   input()
   sys.exit()

try:
   import tkinter as tk
except:
   try:
      import Tkinter as tk
   except:
      print("Import Tkinter library failure.")
      print("An error occurred while trying to import tkinter. Please make sure the tkinter package is installed.\n")
      print("Press any key to end.\n")
      input()
      sys.exit()
      
import ctypes

# Source image path 
image_name = "Seals.mim"
image_path = AIL.M_IMAGE_PATH + image_name
   
class AilEdgeDispTkinter:

   def __init__(self, tk_mainobj, ail_system, ail_display, ail_gralist, ail_image, ail_edgectx, ail_edgeres):
      
      self.ailsystem = ail_system
      self.ailgralist = ail_gralist
      self.ailimage = ail_image
      self.ailedgectx = ail_edgectx
      self.ailedgeres = ail_edgeres
	
      # Retrieve source image sizes 
      self.sizex = AIL.MbufInquire(self.ailimage, AIL.M_SIZE_X)
      self.sizey = AIL.MbufInquire(self.ailimage, AIL.M_SIZE_Y)
      
      # Set tk_mainobj title and define main frames
      tk_mainobj.title("MedgeDispTk Example")
      frame = tk.Frame(tk_mainobj)
      frame.pack()
      
      # Set frames and widgets
      self.frametop = tk.Frame(frame)
      self.frametop.pack(side=tk.TOP, fill=tk.X)
      tk.Label(self.frametop, font="Arial 14", bg="darkgray", fg="white", text="Aurora Imaging Library Tkinter Example", padx=5, pady=5).pack(fill=tk.X)
      
      self.framecenter = tk.Frame(frame, padx=3, pady=3)
      self.framecenter.pack(side=tk.TOP)
      
      self.frameleft = tk.Frame(self.framecenter, padx=3)
      self.frameleft.pack(side=tk.RIGHT)
      
      # Frame to embed a display
      self.framelefttop = tk.Frame(self.frameleft, width=self.sizex, height=self.sizey, bg="", colormap="new")
      self.framelefttop.pack(side=tk.TOP)

      # Associated the display to the frame.
      AIL.MdispSelectWindow(ail_display, ail_image, self.framelefttop.winfo_id())
      
      self.frameright = tk.Frame(self.framecenter)
      self.frameright.pack(side=tk.LEFT)
      
      self.buttonproc = tk.Button(self.frameright, fg="darkgreen", font="Arial 12", text="Extract\nedges", height=2, width=10, command=self.Doprocess)
      self.buttonproc.pack(side=tk.TOP)
      
      self.framebottom = tk.Frame(self.frameleft, width=self.sizex, height=64, pady=10)
      self.framebottom.pack(side=tk.BOTTOM)
		
      self.textresult = tk.Text(self.framebottom, bg="white", height=3, width=50)
      self.textresult.pack()

   def Doprocess(self):
    
      # Extract and draw contours 
      AIL.MedgeCalculate(self.ailedgectx, self.ailimage, AIL.M_NULL, AIL.M_NULL, AIL.M_NULL, self.ailedgeres, AIL.M_DEFAULT)      
      AIL.MedgeDraw(AIL.M_DEFAULT, self.ailedgeres, self.ailgralist, AIL.M_DRAW_EDGES, AIL.M_DEFAULT, AIL.M_DEFAULT)
      
      # Retrieve and outputs the number of found contours 

      number_of_edges = AIL.MedgeGetResult(self.ailedgeres, AIL.M_DEFAULT, AIL.M_NUMBER_OF_CHAINS + AIL.M_TYPE_AIL_INT, None, AIL.M_NULL)
     
      number_of_edges = AIL.AIL_INT(0)
      AIL.MedgeGetResult(self.ailedgeres, AIL.M_DEFAULT, AIL.M_NUMBER_OF_CHAINS + AIL.M_TYPE_AIL_INT, ctypes.byref(number_of_edges), AIL.M_NULL)

      self.textresult.configure(state='normal')
      self.textresult.delete(1.0, tk.END)
      self.textresult.insert(tk.END, "Image : " + image_name + " (" + str(self.sizex) + "x" + str(self.sizey) + ")\n")
      self.textresult.insert(tk.END, "Edges : " + str(number_of_edges.value) + " contours have been found.\n")
      self.textresult.configure(state='disabled')

      
def DoClean():
   # Free objects 
   AIL.MedgeFree(AilEdgeCtx)
   AIL.MedgeFree(AilEdgeRes)   
   AIL.MdispFree(AilDisplay)
   AIL.MgraFree(AilGraList)
   AIL.MbufFree(AilImage)
   AIL.MsysFree(AilSystem)
   AIL.MappFree(AilApplication)
   root.destroy()

# MAIN 
def main():
   
   print("\n[SYNOPSIS]\n")
   print("This program shows how to embed a display\ninto a Tkinter frame using Python language.\n")
   print("Close the displayed window to exit.")
   
   # Allocate objects 
   global AilApplication
   global AilSystem
   global AilDisplay
   global AilImage
   global AilGraList
   global AilEdgeCtx
   global AilEdgeRes
   global root
   
   AilApplication = AIL.MappAlloc("M_DEFAULT", AIL.M_DEFAULT)
   AilSystem = AIL.MsysAlloc(AIL.M_DEFAULT, AIL.M_SYSTEM_DEFAULT, AIL.M_DEFAULT, AIL.M_DEFAULT)
   AilDisplay = AIL.MdispAlloc(AilSystem, AIL.M_DEFAULT, "M_DEFAULT", AIL.M_WINDOWED)
   AilImage = AIL.MbufRestore(image_path, AilSystem)
   AilGraList = AIL.MgraAllocList(AilSystem, AIL.M_DEFAULT)
   AIL.MdispControl(AilDisplay, AIL.M_ASSOCIATED_GRAPHIC_LIST_ID, AilGraList)
   AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, AIL.M_COLOR_RED)
   AilEdgeCtx = AIL.MedgeAlloc(AilSystem, AIL.M_CONTOUR, AIL.M_DEFAULT)
   AilEdgeRes = AIL.MedgeAllocResult(AilSystem, AIL.M_DEFAULT)

   # Start Tk module 
   root = tk.Tk()
   try :
      IcoDir = os.path.dirname(os.path.abspath(__file__))
      IcoFile = os.path.join(IcoDir, 'ail.ico')
      root.iconbitmap(IcoFile)
   except:
       pass
   AilEdgeDispTkinter(root, AilSystem, AilDisplay, AilGraList, AilImage, AilEdgeCtx, AilEdgeRes)
   root.protocol('WM_DELETE_WINDOW', DoClean)	
   root.mainloop()   
   
# Main execution call 
if __name__ == "__main__":
    main()

```
