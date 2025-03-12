# MursaatPanel Plugin API
MursaatPanel offers support for other addons to create widgets for their content to be displayed.

**Note:** The API is in experimental state and is subject to change at any time. Changes will be communicated on the Nexus Discord.

### Definitions
The API consists of the following object and function definitions:
```c++
struct RenderOptions {
    float iconWidth;
    float iconHeight;
    bool renderIcon;
};

typedef void (*RenderFunc)(RenderOptions options);
typedef void (*RenderContextMenuFunc)();
typedef float (*GetWidth)();

struct RegisterWidgetEvent {
    const char* id;
    const char* title;
    RenderFunc render;
    RenderContextMenuFunc renderContextMenu;
    GetWidth getWidth;
};

struct UnregisterWidgetEvent {
    const char* id;
};
```
The idea is for addons to send registration events via Nexus Event API whenever they load in or receive a notification about MursaatPanel being enabled, and to send deregistration events when they are being unloaded again to prevent access to invalid function pointers.

MursaatPanel will register the configuration submitted in the RegisterEvent. Once activated, MursaatPanel will make the following calls each render loop:
- GetWidth(): used to pre-calculate the width of the widget in order to space them out correctly; this will be the width your widget will be assigned on the panel. Value is expected to be a float of positive size
- Render(options): used to actually render the content; MursaatPanel will wrap the content into a Child element, so all you need to care about is the actual content of your addon
  - the passed parameter will hold settings relevant to rendering the content that are in control of MursaatPanel

Additionally, if the user configures the widget by opening the context menu, the function RenderContextMenu() will be called to allow you to configure settings to your specific requirements.

### Widget Lifecycle
To register a widget, MursaatPanel listens to Nexus Events. In addition, whenever MursaatPanel has been loaded and is ready to serve, it will send out a notification event for addons to register their widgets.

The following events are being listened to:
- `EV_MURSAAT_REGISTER_WIDGET`: Expects a pointer to a RegisterWidgetEvent data
- `EV_MURSAAT_DEREGISTER_WIDGET`: Expects a pointer to an UnregisterWidgetEvent data

The following events will be raised by MursaatPanel:
- `EV_MURSAAT_PANEL_READY`: At startup completion, without a payload; if you receive this event, MursaatPanel is telling you to re-send any Register events.

### Known Issues
This section will probably fill up quickly once developers start adapting it.
One thing I am aware of is that grouping widgets is probably not working as expected. Registering multiple widgets into the same group is not supported as of now, but I am looking into possible solutions so expect the register API to be extended.

### Example: Hello Plugin!
Here is a simple implementation for a standalone addon. It defines the required functions and handlers to set up and tear down a simple widget.

**Widget.h**
```c++
@ -0,0 +1,31 @@
#ifndef WIDGET_H
#define WIDGET_H

#include <string>

struct RenderOptions {
    float iconWidth;
    float iconHeight;
    bool renderIcon;
};

typedef void (*RenderFunc)(RenderOptions options);
typedef void (*RenderContextMenuFunc)();
typedef float (*GetWidthFunc)();

struct RegisterWidgetEvent {
    const char* id;
    const char* title;
    RenderFunc render;
    RenderContextMenuFunc renderContextMenu;
    GetWidthFunc getWidth;
};
struct UnregisterWidgetEvent {
    const char* id;
};

void RenderWidget(RenderOptions options);
void RenderContextMenu();
float GetWidth();

#endif
```
**Widget.cpp**
```c++
@@ -0,0 +1,25 @@
#include "Widget.h"
#include "imgui/imgui.h"

bool niceGreeting = true;

std::string GetOutputText() {
  if(niceGreeting) {
	  return "Hello Plugin!";
  } else {
    return "Hello Plugin but not nice!";
  }
}

void RenderWidget(RenderOptions options) {
	ImGui::Text(GetOutputText().c_str());
}
void RenderContextMenu() {
	ImGui::Checkbox("Nice Greeting?", &niceGreeting);
}
float GetWidth() {
	return ImGui::CalcTextSize(GetOutputText().c_str()).x;
}
```
**entry.cpp**
```c++
#include "Widget.h"

void HandleMursaatPanelReady(void* args);

// other content ommited
void AddonLoad(AddonAPI* aApi) {
  // Loading works
  APIDefs->Events.Subscribe("EV_MURSAAT_PANEL_READY", HandleMursaatPanelReady);
  // Register Widget
	HandleMursaatPanelReady(nullptr);

// further loading works
}

void AddonUnload()
{
  // Unregister Widget
  APIDefs->Events.Unsubscribe("EV_MURSAAT_PANEL_READY", HandleMursaatPanelReady);

	UnregisterWidgetEvent* unregisterWidget = new UnregisterWidgetEvent();
	unregisterWidget->id = "hello_widget";
	APIDefs->Events.Raise("EV_MURSAAT_DEREGISTER_WIDGET", unregisterWidget);

  // further unloading works
}

void HandleMursaatPanelReady(void* eventArgs) {
  // send it in a separate thread to avoid deadlocks if triggered by MursaatPanel ready event!!
	std::thread registerThread = std::thread([] {
		RegisterWidgetEvent* registerWidget = new RegisterWidgetEvent();
		registerWidget->id = "hello_widget";
		registerWidget->title = "Hello Widget";
		registerWidget->render = RenderWidget;
		registerWidget->renderContextMenu = RenderContextMenu;
		registerWidget->getWidth = GetWidth;
		APIDefs->Events.Raise("EV_MURSAAT_REGISTER_WIDGET", registerWidget);
	});
	registerThread.detach();
}
