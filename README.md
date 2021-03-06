# LTHPasscodeViewController
Simple to use iOS 7 style Passcode - the one you get in Settings when changing your passcode.

# How to use
Drag the contents of `LTHPasscodeViewController` to your project, or add `pod 'LTHPasscodeViewController'` to your podspec file.

Example, called in `application:didFinishLaunchingWithOptions`:

```objc
[LTHPasscodeViewController useKeychain:NO];
if ([LTHPasscodeViewController doesPasscodeExist]) {
	if ([LTHPasscodeViewController didPasscodeTimerEnd])
		[[LTHPasscodeViewController sharedUser] showLockScreenWithAnimation:YES
                                                                 withLogout:NO
                                                             andLogoutTitle:nil];
}
```

* Supports simple (4 digit) and complex passcodes.
* Data us saved in the Keychain, by default. Supports custom saving, by calling `[LTHPasscodeViewController useKeychain:NO]` after initializing and implementing a few protocol methods (the same names the library uses for the same job):

```objc
- (NSTimeInterval)timerDuration;
- (void)saveTimerDuration:(NSTimeInterval)duration;
- (NSTimeInterval)timerStartTime;
- (void)saveTimerStartTime;
- (BOOL)didPasscodeTimerEnd;
- (void)deletePasscode;
- (void)savePasscode:(NSString *)passcode;
- (NSString *)passcode;
// All of them fall back on the Keychain if they are not implemented, even if [LTHPasscodeViewController useKeychain:NO] was called, for flexibility over what and where you save. 
// Do you only want to save the passcode in a different location and leave everything else in the Keychain? Call [LTHPasscodeViewController useKeychain:NO], but only implement -savePasscode:
```

* Open as a modal, or pushed for changing, enabling or disabling the passcode:

```objc
/**
 @param	viewController The view controller where the passcode view controller will be displayed.
 @param asModal        Set to YES to present as a modal, or to NO to push on the current nav stack.
 */
- (void)showForEnablingPasscodeInViewController:(UIViewController *)viewController asModal:(BOOL)isModal;
- (void)showForDisablingPasscodeInViewController:(UIViewController *)viewController asModal:(BOOL)isModal;
- (void)showForChangingPasscodeInViewController:(UIViewController *)viewController asModal:(BOOL)isModal;
```

* Show the lock screen:

```objc
- (void)showLockScreenWithAnimation:(BOOL)animated withLogout:(BOOL)hasLogout andLogoutTitle:(NSString*)logoutTitle;

// Example:
[[LTHPasscodeViewController sharedUser] showLockscreenWithAnimation:YES withLogout:NO andLogoutTitle:nil];
// Displayed with a slide up animation, which, combined with 
// the keyboard sliding down animation, creates an "unlocking" impression.
```

* entering foreground and resigning is handled from within the class. 


Makes use of [SFHFKeyChainUtils](https://github.com/ldandersen/scifihifi-iphone) to save the passcode in the Keychain. I know he dropped support for it, but I updated it for ARC 2 years ago ([with help](http://stackoverflow.com/questions/7663443/sfhfkeychainutils-ios-keychain-arc-compatible)) and I kept using it since. The 'new' version isn't updated to ARC anyway, so I saw no reason to switch to it, or to any other library.

Feel free to [contact me](mailto:roland@rolandleth.com), or open an issue if anything is unclear, bugged, or can be improved. 

![Screenshot](http://rolandleth.com/assets/ios7-style-passcode/screenshot.png)   ![Screenshot](http://rolandleth.com/assets/ios7-style-passcode/change-passcode-screenshot.png)

# Apps using this control
[Expenses Planner](https://itunes.apple.com/us/app/expenses-planner-reminders/id669431471?mt=8)  
[DigitalOcean Manager](https://itunes.apple.com/us/app/digitalocean-manager/id633128302?mt=8)  
[LovelyHeroku](https://itunes.apple.com/us/app/lovelyheroku/id706287663?mt=8&uo=4)  
[Flow Web Browser](https://itunes.apple.com/us/app/flow-web-browser-downloader/id705536564?mt=8)  
[Balance - Checkbook App](https://itunes.apple.com/US/app/id854362248)
[QIF Reader](https://itunes.apple.com/us/app/qif-reader/id374178932?mt=8)

If you're using this control, I'd love hearing from you!  

# License
Licensed under MIT. If you'd like (or need) a license without attribution, don't hesitate to [contact me](mailto:roland@rolandleth.com).
