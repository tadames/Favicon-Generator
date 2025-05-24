#!/usr/bin/env python3

"""
Favicon Generator Script (Python)
Converts any image to favicon.ico with multiple sizes

Usage: python generate_favicon.py <input-image-path> [output-directory]
Example: python generate_favicon.py logo.png ./public
"""

import sys
import os
from PIL import Image, ImageOps
import json

# Favicon sizes typically included in .ico files
FAVICON_SIZES = [16, 32, 48, 64, 128, 256]
ANDROID_SIZES = [192, 512]
APPLE_ICON_SIZE = 180

def generate_favicon(input_path, output_dir='./public'):
    """Generate favicon and related files from input image"""
    
    # Validate input file
    if not os.path.exists(input_path):
        raise FileNotFoundError(f"Input file not found: {input_path}")
    
    # Create output directory
    os.makedirs(output_dir, exist_ok=True)
    
    print(f"🚀 Generating favicon from: {input_path}")
    print(f"📁 Output directory: {output_dir}")
    
    # Open and process the source image
    try:
        with Image.open(input_path) as img:
            # Convert to RGBA if not already
            if img.mode != 'RGBA':
                img = img.convert('RGBA')
            
            # Generate individual PNG files for each size
            images_for_ico = []
            
            for size in FAVICON_SIZES:
                # Resize with high quality
                resized = img.resize((size, size), Image.Resampling.LANCZOS)
                
                # Save individual PNG
                png_path = os.path.join(output_dir, f'favicon-{size}x{size}.png')
                resized.save(png_path, 'PNG', optimize=True)
                
                # Keep for ICO creation
                images_for_ico.append(resized.copy())
                
                print(f"✅ Generated {size}x{size} PNG")
            
            # Create favicon.ico with multiple sizes
            ico_path = os.path.join(output_dir, 'favicon.ico')
            images_for_ico[0].save(
                ico_path,
                format='ICO',
                sizes=[(size, size) for size in FAVICON_SIZES],
                append_images=images_for_ico[1:]
            )
            print("🎉 Successfully created favicon.ico!")
            
            # Generate Apple Touch Icon
            apple_icon = img.resize((APPLE_ICON_SIZE, APPLE_ICON_SIZE), Image.Resampling.LANCZOS)
            apple_path = os.path.join(output_dir, 'apple-touch-icon.png')
            apple_icon.save(apple_path, 'PNG', optimize=True)
            print(f"✅ Generated Apple Touch Icon ({APPLE_ICON_SIZE}x{APPLE_ICON_SIZE})")
            
            # Generate Android Chrome icons
            for size in ANDROID_SIZES:
                android_icon = img.resize((size, size), Image.Resampling.LANCZOS)
                android_path = os.path.join(output_dir, f'android-chrome-{size}x{size}.png')
                android_icon.save(android_path, 'PNG', optimize=True)
                print(f"✅ Generated Android Chrome icon ({size}x{size})")
            
            # Generate web manifest
            generate_web_manifest(output_dir)
            
            print_html_instructions()
            
    except Exception as e:
        print(f"❌ Error processing image: {e}")
        sys.exit(1)

def generate_web_manifest(output_dir):
    """Generate site.webmanifest file"""
    manifest = {
        "name": "Circus Tactics",
        "short_name": "CircusTactics", 
        "icons": [
            {
                "src": "/android-chrome-192x192.png",
                "sizes": "192x192",
                "type": "image/png"
            },
            {
                "src": "/android-chrome-512x512.png", 
                "sizes": "512x512",
                "type": "image/png"
            }
        ],
        "theme_color": "#39ff14",
        "background_color": "#111111",
        "display": "standalone"
    }
    
    manifest_path = os.path.join(output_dir, 'site.webmanifest')
    with open(manifest_path, 'w') as f:
        json.dump(manifest, f, indent=2)
    
    print("✅ Generated site.webmanifest")

def print_html_instructions():
    """Print HTML instructions for using the generated files"""
    print('\n🎯 Favicon generation complete!')
    print('\nAdd these to your HTML <head>:')
    print('<link rel="icon" type="image/x-icon" href="/favicon.ico">')
    print('<link rel="icon" type="image/png" sizes="32x32" href="/favicon-32x32.png">')
    print('<link rel="icon" type="image/png" sizes="16x16" href="/favicon-16x16.png">')
    print('<link rel="apple-touch-icon" sizes="180x180" href="/apple-touch-icon.png">')
    print('<link rel="manifest" href="/site.webmanifest">')

def main():
    """Main CLI function"""
    if len(sys.argv) < 2:
        print("""
🎨 Favicon Generator (Python)

Usage: python generate_favicon.py <input-image> [output-directory]

Examples:
  python generate_favicon.py logo.png
  python generate_favicon.py logo.svg ./public
  python generate_favicon.py image.jpg ./static

Supported input formats: PNG, JPG, JPEG, SVG, WEBP, GIF, BMP
        """)
        sys.exit(1)
    
    input_path = sys.argv[1]
    output_dir = sys.argv[2] if len(sys.argv) > 2 else './public'
    
    try:
        generate_favicon(input_path, output_dir)
    except Exception as e:
        print(f"❌ Error: {e}")
        sys.exit(1)

if __name__ == '__main__':
    main()
