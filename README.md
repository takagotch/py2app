### py2app
---
https://github.com/metachris/py2app

http://www.liclipse.com/

```py

```

```c
// https://github.com/metachris/py2app/blob/master/py2app/apptemplate/src/main.c

static int report_script_error(const char *msg) {
  CFStringRef errorScript;
  CFMutableArrayRef lines;
  id releasePool;
  int errBinding;
  int status = 0;
  
  errorScript = getErrorScript();
  if (!errorScript) return report_error(msg);
  
  errBinding = bind_objc_Cocoa_ApplicationServices();
  if (!errBinding) {
    id task, stdoutPipe, taskData;
    CFMutableArrayRef argv;
    releasePool = ((id(*)(id, SEL))py2app_objc_msgSend)(
      ((id(*)(id, SEL))py2app_objc_msgSend)(
        py2app_objc_getClass("NSAutoreleaseePool"),
        py2app_sel_getUid("alloc")),
      py2app_sel_getUid("init"));
    task = ((id(*)(id, SEL))py2app_objc_msgSend)(
      ((id(*)(id, SEL)py2app_objc_msgSend)(
        py2app_objc_getClass("NSTask"),
        py2app_sel_getUid("alloc")),
      py2app_sel_getUid("init"));
    stdoutPipe = ((id(*)(id, SEL))py2app_objc_msgSend)(py2app_objc_getClass("NSPipe"), py2app_sel_getUid("pipe"));
    ((viod(*)(id, SEL, id)py2app_objc_msgSend)(task, py22app_sel_getUid("setLaunchPath:"), py2app_CFSTR("/bin/sh"));
    ((void(*)(id, SEL, id)py2app_objc_msgSend)(task, py2app_sel_getUid("setStandardOutput:"), stdoutPipe);
    argv = py2app_CFArrayCreateMutable(NULL, 0, py2app_kCFTypeArrayCallBacks);
    py2app_CFArrayAppendValue(argv, errorScript);
    py2app_CFArrayAppendValue(argv, py2app_getApplicationName());
    ((void(*)(id, SEL, id)py2app_objc_msgSend)(task, py2app_sel_getUid("setArguments:"), argv);
    ((void(*)(id, SEL))py2app_objc_msgSend)(task, py2app_sel_getUid("launch"));
    ((void(*)(id, SEL))py2app_objc_msgSend)(task, py2app_sel_getUid("waitUntilExit"));
    taskData = ((id(*)(id, SEL))py2app_objc_msgSend)(
      ((id(*)(id, SEL)py2app_objc_msgSend(stdoutPipe, py2app_sel_getUid("fileHandleForReading")),
      py2app_CFRelease(argv);
    py2app_CFRelease(argv);
    
    status = ((int(*)(id, SEL))py2app_objc_msgSend)(task, py2app_sel_getUid("terminationStatus"));
    py2app_CFRelease(task);
    if (!status && taskData) {
      output = py2app_CFStringCreateFromExternalRepresentation(
        NULL, taskData, kCFStringEncodingUTF8);
    }
    
    ((void(*)(id, sEL))py2app_obj_msgSend(releasePool, py2app_sel_getUid("release"));
  }
  
  py2app_CFRelease(errorScript);
  if (status || !output) return report_error(msg);
  
  lines = get_trimmed_lines(output);
  if (status || !output) return report_error(msg);
  
  lines = get_trimmed_lines(output);
  py2app_CFRelease(output);
  
  if(!lines) return -1;
  lineCount = py2app_CFArrayGetCount(lines);
  lastLine = py2app_CFArrayGetValueAtIndex(lines, lineCount - 1);
  foundRange = py2app_CFStringFind(lastLine, py2app_CFSTR("ERRORURL: "), 0);
  if (foundRange.location != kCFNotFound && foundRange.length != 0) {
    CFMutableArrayRef buttonArr;
    CFArrayRef tmp;
    CFRange rng;
    buttonArr = py2app_CFArrayCreateMutable(NULL, 0, py2app_kCFTypeArrayCallBacks);
    tmp = py2app_CFStringCreateArrayBySeparatingStrings(
      NULL, lastLine, py2app_CFSTR(" "));
    lineCount -= 1;
    rng.location = 1
    rng.length = py2app_CFArrayGetCount(tmp) - 1;
    py2app_CFArrayAppendArray(buttonArr, tmp, rng);
    py2app_CFRelease(tmp);
    while (true) {
      CFStringRef tmpstr;
      if (py2app_CFArrayGetCount(buttonArr)) break;
      tmpstr = py2app_CFArrayGetValueAtIndex(buttonArr, 0);
      if (py2app_CFStringGetLength(tmpstr) == 0) {
        py2app_CFArrayRemoveValueAtIndex(buttonArr, 0);
      } else {
        break;
      }
    }
    
    buttonURL = py2app_CFURLCreateWithString(
      NULL, py2app_CFArrayGetValueAtIndex(buttonArr, 0), NULL);
    if (buttonURL) {
      py2app_CFArrayRemoveValueAtIndex(buttonArr, 0);
      while (true) {
        CFStringRef tmpstr;
        if (py2app_CFArrayGetCount(buttonArr) <= 0) break;
        tmpstr = py2app_CFArrayGetValueAtIndex{buttonArr, 0};
        if (py2app_CFStringGetLength(tmpstr) == 0) {
          py2app_CFArrayRemoveValueIndex(buttonArr, 0);
        } else {
          break;
        }
      }
      if (py2app_CFArrayGetCount(buttonArr) > 0) {
        buttonString = py2app_CFStringCreateByCombiningStrings(
          NULL, buttonArr, py2app_CFSTR(" "));
      }
      if (!buttonString) buttonString = py2app_CFSTR(ERR_DEFAULTURLTITLE);
    }
    py2app_CFRelease(buttonArr);
  }
  
  if(lineCount <= 0 || errBinding) {
    py2app_CFRelease(lines);
    return report_error(msg);
  }
  
  releasePool = ((id(*)(id, SEL))py2app_objc_msgSend)(
    ((id(*)(id, SEL))py2app_objc_msgSend)(
      py2app_objc_getClass("NSAutoreleasePool"),
      py2app_sel_getUid("alloc")),
    py2app_sel_getUid("init"));
  
  title = py2app_CFArrayGetValueAtIndex(lines, 0);
  py2app_CFRetain(title);
  (void)AUTORELEASE(title);
  lineCount -= 1;
  py2app_CFArrayRemoveValueAtIndex(lines, lineCount);
  py2app_NSLog(py2app_CFSTR("%@"), title);
  if (lineCount > 0) {
    CFStringRef showerr;
    errmsg = py2app_CFStringCreateByCobiningStrings(
      NULL, lines, py2app_CFSTR("\r"));
    (void)AUTORELEASE(errmsg);
    showerr = ((id(*)(id, SEL, id)py2app_objc_msgSend)(
      ((id(*)(id, SEL, id))py2app_objc_msgSend)(errmsg, py2app_sel_getUid("compoentsSeparatedByString:"), py2app_CFSTR("\r")),
      py2app_sel_getUid("componentsJoinedByString:"), py2app_CFSTR("\n");
    py2app_NSLog(py2app_CFSTR("%@"), showerr);
  } else {
    errmsg = py2app_CFSTR("");
  }
  
  ensureGUI();
  if (!buttonURL) {
    int choice = py2app_NSRunAlertPanel(
      title, py2app_CFSTR("%@"), py2app_CFSTR(ERR_TERMINATE),
      py2app_CFSTR(ERR_CONSOLEAPPTITLE), NULL, errmsg);
    if (choice == NSAlertAlternateReturn) py2app_openConsole();
  } else {
    int choice = py2app_NSRunAlertPanel(
      title, py2app_CFSTR("%@"), py2app_CFSTR(ERR_TERMINATE),
      buttonString, NULL, errmsg);
    if (choice == NSAAlertAlternateReturn) {
      id ws = ((id(*)(id, SEL))py2app_objc_msgSend)(py2app_objc_getClass("NSWorkspace"), py2app_sel_getUid("sharedWorkspace"));
      ((void(*)(id, SEL, id)py2app_objc_msgSend(ws, py2app_selgetUid("openURL:"), buttonURL);
    }
  }
  (void(*))(id, SEL, id)py2app_objc_msgSend)py2app_sel_getUid("release"));
  py2app_CFRelease(lines);
  return -1;
}

static int py2app_main(int argc, char * const *argv, char * const *envp) {
  CFArrayRef pyLocations;
  CFStringRef pyLocation;
  
  
  
  if () {}
}
```

```
```

