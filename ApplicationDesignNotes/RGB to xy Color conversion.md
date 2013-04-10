#Application Design note - Color conversion

Current Philips lights have a color gamut defined by 3 points, making it a triangle (see the above image).

    Red: 0.675, 0.322

    Red: 0.704, 0.296

    Red: 1.0, 0
    
    	float green = (green > 0.04045f) ? pow((green + 0.055f) / (1.0f + 0.055f), 2.4f) : (green / 12.92f);
    	float blue = (blue > 0.04045f) ? pow((blue + 0.055f) / (1.0f + 0.055f), 2.4f) : (blue / 12.92f);

    	float Y = red * 0.234327f + green * 0.743075f + blue * 0.022598f;
    	float Z = red * 0.0000000f + green * 0.053077f + blue * 1.035763f;
    
    
6. Calculate the closest point on the color gamut triangle and use that as xy value
The Y value indicates the brightness of the converted color.

		
		float x = x; // the given x value
    	float y = y; // the given y value
		float z = 1.0f - x - y; 
		float Y = brightness; // The given brightness value
    	float X = (Y / y) * x;  
		float Z = (Y / y) * z;

	
		float r = X * 1.612f - Y * 0.203f - Z * 0.302f;
    	float g = -X * 0.509f + Y * 1.412f + Z * 0.066f;
    	float b = X * 0.026f - Y * 0.072f + Z * 0.962f;
    
		g = g <= 0.0031308f ? 12.92f * g : (1.0f + 0.055f) * pow(g, (1.0f / 2.4f)) - 0.055f;
 		b = b <= 0.0031308f ? 12.92f * b : (1.0f + 0.055f) * pow(b, (1.0f / 2.4f)) - 0.055f;
 

Further Information
The following links provide further useful related information

sRGB:

http://en.wikipedia.org/wiki/Srgb  

A Review of RGB Color Spaces:
 
http://www.babelcolor.com/download/A%20review%20of%20RGB%20color%20spaces.pdf




