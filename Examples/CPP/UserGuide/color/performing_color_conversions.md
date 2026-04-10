---
title: "performing_color_conversions"
description: "RGB to HLS conversion algorithm"
ms.snippet: "userguide.color.performing_color_conversions"
ms.language: "C++"
ms.env: "vc"
ms.version: "7.5"
---

# performing_color_conversions

> RGB to HLS conversion algorithm

**Language:** C++ | **Version:** 7.5+

```cpp
#define min(a, b) (((a) < (b)) ? (a) : (b))
#define max(a, b) (((a) > (b)) ? (a) : (b))

// Input and output between 0 and 1.

void RGBToHSL(float R, float G, float B, float* H, float* S, float* L)
   {
   float MinVal, MaxVal, Delta;

   // Min and max values.
   MaxVal = max(R, max(G, B));
   MinVal = min(R, min(G, B));

   Delta = MaxVal - MinVal;

   // Compute the luminance.
   *L = 0.5f * (MaxVal + MinVal);

   if (Delta == 0.0f)
      {
      *S = *H = 0.0f;
      }
   else
      {
      // Compute the saturation.
      if (*L <= 0.5f)
         {
         *S = Delta / (MaxVal + MinVal);
         }
      else
         {
         *S = Delta / (2.0f - MaxVal - MinVal);
         }

      // Compute the hue.
      if (MaxVal == R)
         {
         *H = 60.0f * ((G - B) / Delta);
         }
      else if (MaxVal == G)
         {
         *H = 60.0f * (2.0f + ((B - R) / Delta));
         }
      else
         {
         *H = 60.0f * (4.0f + ((R - G) / Delta));
         }

      if (*H < 0.0f)
         {
         *H += 360.0f;
         }
      }

   // Remap the angle between 0 and 1.

   *H = *H / 360.0f;
   }

float HueToRGB(float Temp1, float Temp2, float Hue)
   {
   if (Hue > 360.0f)
      {
      Hue = Hue - 360.0f;
      }
   else if (Hue < 0.0f)
      {
      Hue = Hue + 360.0f;
      }

   if (Hue < 60.0f)
      {
      return (Temp1 + (Temp2 - Temp1) * Hue / 60.0f);
      }
   else if (Hue < 180.0f)
      {
      return Temp2;
      }
   else if (Hue < 240.0f)
      {
      return (Temp1 + (Temp2 - Temp1) * (240.0f - Hue) / 60.0f);
      }

   return Temp1;
   }

// Input and output between 0 and 1.

void HSLToRGB(float H, float S, float L, float* R, float* G, float* B)
   {
   float Temp1, Temp2;

   // Remap the angle between 0 and 360.
   H = H * 360.0f;

   // Achromatic case.
   if (S == 0.0f)
      {
      *R = *G = *B = L;
      }
   else
      {
      if (L <= 0.5f)
         {
         Temp2 = L * (1.0f + S);
         }
      else
         {
         Temp2 = L + S - (L * S);
         }

      Temp1 = 2.0f * L - Temp2;

      *R = HueToRGB(Temp1, Temp2, H + 120.0f);
      *G = HueToRGB(Temp1, Temp2, H);
      *B = HueToRGB(Temp1, Temp2, H - 120.0f);
      }
   }
```
