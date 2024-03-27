# Run UWP apps under custom shell

## What is this for?

In Windows 8+ UWP apps [needs PackageIdentity](https://blogs.windows.com/windowsdeveloper/2019/10/29/identity-registration-and-activation-of-non-packaged-win32-apps/) and cannot be directly launched the Win32 way ([example](https://poetengineer.postach.io/post/how-to-open-windows-modern-app-from-the-command-line]). If you use a custom shell then basically all UWP apps cannot run. Since Windows 8 Microsoft moves some configuration options, including even volume and wireless controls to UWP-based apps, a custom shell on generic devices becomes more and more useless.

This guide enables you to run UWP apps, aka [the appendix of Windows applications](https://www.thurrott.com/dev/206351/microsoft-confirms-uwp-is-not-the-future-of-windows-apps) as officially designated, on a custom shell.

## Benefits

If you desire to run a custom shell on *Windows* of all OSes you'd probably already know.

Also, in Windows 11 the conventional Explorer shell are less and less usable (search ``Windows 11 Explorer bugs'' for more), so why not give custom shells a try, as long as you know the consequences?

## Steps

Official guide: https://learn.microsoft.com/en-us/windows/iot/iot-enterprise/customize/shell-launcher
Official config example: https://github.com/microsoft/Windows-iotcore-samples/blob/develop/Samples/ShellLauncherV2/SampleConfigXmls/README.md

1. Make sure you are on Enterprise / Education where the product policy `EmbeddedFeature-ShellLauncher-Enabled` is set to 1.
2. Go to ``Programs and Features'' in the Control Panel and enable the shell launcher per the official guide.
3. Edit the configuration script in the official guide to set the custom shell.
  * For BBLean, set it as `C:\bblean\blackbox.exe`.
  * However, it seems that non-Microsoft-signed `.exe` won't be run. Use a wrapper batch file instead.
  * You have to call `$ShellLauncherClass.RemoveCustomShell()` first after setting a custom shell. Otherwise an exception will be thrown.
4. Log out and log in. If the custom shell isn't running, try launching it using the Task Manager.
5. Use `Win + I` to test it. `ImmersiveControlPanel` should work.

Windows 11 untested but should work.

## Things supposed to work

* Basically all UWP apps, including `Microsoft Store`, UWP-based IMEs and Windows Defender dashboard.
* Modern (i.e. decent-looking) Alt-Tab and Win-Tab screens.
* Virtual desktops.
* Wi-Fi switcher, volume switcher, and various popups triggered by the Fn keys.

## Known Problems

* In BBLean, when you press the Windows key / `Ctrl + Esc` it gives an unclickable Start Menu.
  * Use AutoHotkey.
* Notifications do not work.
* Can't find the wifi and volume tray icons.
  * Seems that `CustomShellHost.exe` subsumed it while it is running.
  * Use `ImmersiveControlPanel -> Network and Internet -> Wi-Fi -> Show Available Networks` for Wi-Fi. Or install an alternative Wi-Fi manager application.
  * Use good old Windows (tm) Mobile Center (`mblctr.exe`).
  * Or use [PE Network Manager](https://www.penetworkmanager.de/).
* Triple click ineffective under alternative shell.
  * Seems like an override when using `CustomShellHost`.
  * Maybe it's due to lack of attention. Maybe the triple / quadruple clicks in recent Windows 10 versions are just forwarded to the shell to process, and running `bbLean` without `CustomShellHost` wrapper does not resolve the issue.
  * Use [my enabler](https://github.com/a-l-r1/windows-custom-shell-three-finger-tap-enabler).
* Precision touchpad slide to switch virtual desktops functionality / show task view conflicts with bbLean.
  * Use just one kind of functionality.

## See Also

https://winclassic.net/thread/1652/list-metro-ified-components-windows (if you prefer non-metro Apps and just run the custom shell plainly without UWP components)
