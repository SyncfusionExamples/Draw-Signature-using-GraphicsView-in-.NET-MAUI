# Draw-Signature-using-GraphicsView-in-.NET-MAUI

This sample demonstrates how to capture a handwritten signature inside a .NET MAUI application using the cross-platform `GraphicsView` control. The `GraphicsView` provides a drawable canvas that can respond to touch and pointer input, making it ideal for implementing signature pads, sketching surfaces, annotation tools, and any other free-hand drawing scenarios on iOS, Android, macOS (via Mac Catalyst), and Windows.

## Overview

The .NET MAUI `GraphicsView` is a versatile view that hosts an `IDrawable` and renders it through the `Microsoft.Maui.Graphics` APIs. By combining `GraphicsView` with touch and pointer events, you can build a fully functional signature pad without relying on platform-specific code. This example walks through the complete implementation: creating a custom drawable to store and render the signature strokes, wiring up touch/pointer events to translate finger or stylus movements into path data, and exposing commands to clear or export the captured signature.

## Prerequisites

To run this sample, you need the following installed on your development machine:

- Visual Studio 2026 (18.0 or later) with the **.NET Multi-platform App UI development** workload.
- .NET 6.0 SDK (or a newer Long-Term Support release supported by .NET MAUI).
- A configured build target for Android, iOS, Mac Catalyst (macOS only), or Windows.
- For iOS/Mac Catalyst builds: Xcode 15 or later installed on the Mac.
- For Android builds: an Android SDK and an emulator or physical device.

## Project Structure

The solution is organized into a single .NET MAUI project that contains:

- `SignatureView.cs` – The custom `GraphicsView` subclass that captures pointer events, accumulates the signature path, and triggers a redraw whenever a new stroke is recorded. It also exposes a `Clear` method to reset the canvas.
- `MainPage.xaml` and `MainPage.xaml.cs` – The page that hosts the `SignatureView`, along with a **Clear** button to erase the canvas and a sample label describing the gesture.
- `App.xaml`, `App.xaml.cs`, and `AppShell.xaml` – Standard .NET MAUI application bootstrap and navigation shell.
- `MauiProgram.cs` – The application entry point that configures fonts, services, and the MAUI builder pipeline.
- `Platforms/` – Platform-specific resources and entry points for Android, iOS, Mac Catalyst, Tizen, and Windows.
- `Resources/` – Fonts, images, splash screen, and shared styles used throughout the app.

## How the Signature Capture Works

1. **Custom `IDrawable`**: A small drawable class keeps a list of polylines (one per stroke). When the framework calls its `Draw` method, it iterates through the stored paths and renders them using a `Path` builder, producing smooth, anti-aliased strokes.
2. **Touch/pointer capture**: The `SignatureView` listens for `StartInteraction`, `MoveInteraction`, and `EndInteraction` events. Each event receives a `Point` in the view's coordinate space, which is appended to the active stroke.
3. **Invalidation**: After every move event, the view calls `Invalidate()`, prompting the `GraphicsView` to redraw the canvas with the updated path data.
4. **Clear operation**: The `Clear` method empties the stored paths and invalidates the view, restoring a blank canvas ready for a new signature.

## Running the Sample

1. Clone or download this repository to your local machine.
2. Open the `2DGraphicsDrawing.sln` file in Visual Studio 2026.
3. Restore the NuGet packages by right-clicking the solution and selecting **Restore NuGet Packages**.
4. Select your desired target framework from the run configuration dropdown (for example, `net6.0-android`, `net6.0-ios`, or `net6.0-windows10.0.19041.0`).
5. Choose an emulator, simulator, or connected device, then press **F5** (or click the run button) to build and deploy the application.
6. Draw on the canvas with your finger, stylus, or mouse to create a signature, then tap **Clear** to start over.

## Key Concepts Illustrated

- **Cross-platform drawing surface**: Using `GraphicsView` as a single API for drawing across all .NET MAUI targets.
- **Microsoft.Maui.Graphics primitives**: Creating `Path` objects, applying stroke thickness, line caps, and joins, and rendering them through `ICanvas`.
- **Pointer interaction handling**: Converting user input into a series of points and persisting them as a drawable model.
- **MVVM-friendly separation**: Keeping drawing state inside a reusable view that can be embedded in any page or view model.

## Extending the Sample

Once the basic signature pad is working, you can extend the project in several useful directions:

- **Save the signature as an image**: Render the drawable to a `Microsoft.Maui.Graphics.Platform.PlatformImage` and write it to disk or stream it to a backend service.
- **Add export options**: Provide buttons to save the signature as PNG or SVG, copy it to the clipboard, or share it via the system share sheet.
- **Stroke customization**: Expose properties for stroke color, thickness, and smoothing so users can personalize their signature.
- **Undo/redo support**: Maintain a history of strokes and add commands to step backwards or forwards through the captured paths.
- **Validation**: Enforce a minimum number of strokes or points before allowing a signature to be submitted.

## Troubleshooting

- If the project fails to build, ensure the .NET MAUI workload is installed by running `dotnet workload install maui` from a terminal.
- On Windows, confirm that the **Windows App SDK** and required Visual Studio components are present.
- For Android emulator issues, verify that hardware acceleration is enabled and that the emulator image matches your project's target framework.
- If touch input is not registered, make sure the `SignatureView` is not wrapped inside a scrollable container that might intercept the gestures.

## Blog Reference

For a detailed walkthrough of the concepts used in this sample, refer to the Syncfusion blog post:

[How to Draw 2D Graphics in .NET MAUI’s GraphicsView](https://www.syncfusion.com/blogs/post/draw-2d-graphics-in-dotnet-maui-graphicsview.aspx)

## Related Resources

- [.NET MAUI documentation](https://learn.microsoft.com/dotnet/maui/)
- [Microsoft.Maui.Graphics overview](https://learn.microsoft.com/dotnet/maui/user-interface/graphics/)
- [Syncfusion .NET MAUI controls](https://www.syncfusion.com/maui-controls)

## License

This sample is provided as a learning resource alongside the Syncfusion blog post. Refer to the repository's license file for redistribution terms.
