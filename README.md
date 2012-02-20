PPRevealSideViewController
==========================

This is a new controller container, showing views on the side like the Facebook or Path app. It is as easy to use as a navigation controller.
Sometimes, you need to push a new controller to show some options, but a small controller would be enough … PPRevealSideViewController is the controller you need.

# Installation

1. In your XCode Project, take the *PPRevealSideViewController.h and .m* from PPRevealSideViewController folder and drag them into your project. 
2. Add *PPRevealSideViewController.h* file to your PCH file or your AppDelegate file.
3. Add the QuartzCore Framework.
4. Start using this new controller!

# ARC Support

PPRevealSideViewController fully supports ARC *and* non-ARC modes out of the box, there is no configuration necessary. This is really convenient when you have older projects, or do no want to use ARC.  ARC support has been tested with the Apple LLVM 3.0 compiler.

# Usage

## Creating your controller
Somewhere, for example in your app delegate, alloc and init the controller :

    _revealSideViewController = [[PPRevealSideViewController alloc] initWithRootViewController:rootController];

Then, add it to the window

	self.window.rootViewController = _revealSideViewController;

or to a view 

	[self.view addSubview:_revealSideViewController.view];
	

You can add a navigation controller on top, for example : 

	MainViewController *main = [[MainViewController alloc] initWithNibName:@"MainViewController" bundle:nil];
    UINavigationController *nav = [[UINavigationController alloc] initWithRootViewController:main];
    _revealSideViewController = [[PPRevealSideViewController alloc] initWithRootViewController:nav];
    
    self.window.rootViewController = _revealSideViewController;
    
## Pushing a controller
You have several options to push a controller (see the documentation or the sample)
The easiest way is : 

	PopedViewController *c = [[PopedViewController alloc] initWithNibName:@"PopedViewController" bundle:nil ];
    [self.revealSideViewController pushViewController:c onDirection:PPRevealSideDirectionBottom animated:YES];

This will push the controller on bottom, with a default offset.
You have four directions : 

	PPRevealSideDirectionBottom
	PPRevealSideDirectionTop
	PPRevealSideDirectionLeft
	PPRevealSideDirectionRight
	
## Popping
To go back to your center controller from a side controller, you can pop :

    [self.revealSideViewController popViewControllerAnimated:YES];

If you want to pop a new center controller, then do the following :

	MainViewController *c = [[MainViewController alloc] initWithNibName:@"MainViewController" bundle:nil];
    UINavigationController *n = [[UINavigationController alloc] initWithRootViewController:c];
    [self.revealSideViewController popViewControllerWithNewCenterController:n animated:YES];
    
## To go deeper 
By default, the side views are not loaded. This means that even if you interface have a button to push a side view, the panning gesture won't show the controller. If you want so, you need to preload the controller you want to present.

	TableViewController *c = [[TableViewController alloc] initWithStyle:UITableViewStylePlain];
    [self.revealSideViewController preloadViewController:c
                                                 forSide:PPRevealSideDirectionLeft
                                              withOffset:_offset];
                                              
Please not that there can be some interferences with the preload method, when you pop a center controller with a preload controller on the same side that the one you pop from… For that reason, I highly recommand you to delay the preload method. For example : 

	- (void) viewDidAppear:(BOOL)animated {
	    [super viewDidAppear:animated];
	    [NSObject cancelPreviousPerformRequestsWithTarget:self selector:@selector(preloadLeft) object:nil];
	    [self performSelector:@selector(preloadLeft) withObject:nil afterDelay:0.3];
	}
	
## Options
You have some options availabled defined in PPRevealSideOptions

* PPRevealSideOptionsShowShadows : Disable or enable the shadows. Enabled by default.
* PPRevealSideOptionsBounceAnimations : Decide if the animations are boucing or not. By default, they are.
* PPRevealSideOptionsCloseCompletlyBeforeOpeningNewDirection : Decide if we close completely the old direction, for the new one or not. Set to YES by default.
* PPRevealSideOptionsKeepOffsetOnRotation : Keep the same offset when rotating. By default, set to no.
* PPRevealSideOptionsResizeSideView : Resize the side view. If set to yes, this disabled the bouncing stuff since the view behind is not large enough to show bouncing correctly. Set to NO by default.

                                                
License
-------

This Code is released under the MIT License by [Marian Paul for iPuP SARL](http://www.ipup.pro)

Please tell me when you use this controller in your project !

Regards,  
Marian Paul aka ipodishima
  
http://www.ipup.pro
