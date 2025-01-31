---
layout: default
seo_title: RubyMotion Applied
heading_baseline: An example/tutorial based approach to learning mobile development with Ruby.
---

This page contains documentation that will help you get started with
building mobile apps using Ruby. Content on this page is released under
Apache 2.0 and can be found [here](http://github.com/amirrajan/rubymotion-applied).
Feel free to contribute your own documentation or provide suggestions
via GitHub Issues.

# Guidelines #

1. Join the [RubyMotion Slack Channel](https://motioneers.herokuapp.com/).
2. These tutorials should be done in order.
3. The tutorials are ordered in increasing difficulty, by platform:
   1. iPhone/iPad
   1. Android
   1. Cross Platform Mobile
   1. Non-mobile Development: MacOS, tvOS, WatchOS (requires at least a RubyMotion Indie License)
   1. Game Development: iPhone, iPad, Android, and Cross Platform
   1. The RubyMotion Runtime itself
4. Tutorials target the latest stable iOS/Android releases (unless stated otherwise).
5. The [WNDX School](https://wndx.school) (paid) has some more in-depth video courses. Courses in beta are currently free.
   1. [Introduction to iPhone Development with RubyMotion](https://wndx.school/p/introduction-to-iphone-development-with-rubymotion)

# Jump To Tutorial #

## iPhone/iPad Development Tutorials ##

1. [Getting Your Mac Set Up for iOS Development](#tutorial1)
1. [Wrapping a Responsive Website and Deploying to the AppStore](#todo)
1. [Adding a `UIButton` programmatically and wiring it up to do something when tapped](#tutorial3)
1. [Working with the `UINavigationController`](#todo)
1. [Working with the `UITableViewController`](#todo)
1. [Building Layouts using Masonry](#todo)
1. [Interacting with JSON APIs AFNetworking](#todo)
1. [Working with Isolated Storage](#todo)
1. [IAP/Monetization](#todo)
1. [Push Notifications](#todo)
1. [RubyMotion Tutorial for Objective C Developers](http://hboon.com/rubymotion-tutorial-for-objective-c-developers/)

## Android Development Tutorials ##

1. [Getting Your Mac Set Up for Android Development]()

## Cross Platform Mobile Tutorials ##

1. [Understanding the Challenges for Building Crossplatform Apps]()

## Non-mobile Development Tutorials ##

1. [Todo]()

## Game Development Tutorials ##

1. [Todo]()

## RubyMotion Runtime Tutorials

NOTE: RubyMotion is currently closed source, but will become more open
with time. These tutorials will give you insight into how RubyMotion
works behind the scenes, and what you'd need to learn in order to work
on RubyMotion (as the source code becomes more open). These are fairly
advanced tutorials, and are not required to build apps with RubyMotion.

1. [RubyMotion Rake Tasks Demystified]()
1. [Creating Foreign Function Interfaces to C++ Libraries using `rubmotion.h`]()
1. [Setting Up Your Mac to Build RubyMotion from Source]()
1. [LLVM Crash Course]()

<hr />

# iPhone/iPad Development Tutorials #

<span id="tutorial1"></span>

# Getting Your Mac Set Up For iOS Development #

TODO

## Installing Homebrew ##

Paste this at a terminal prompt
```
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```
[Source](https://brew.sh/)

## Installing rbenv or rvm ##

### rvm

Paste this at a terminal prompt


```
curl -sSL https://get.rvm.io | bash -s stable --ruby
```

## Installing Xcode and CLI Tools to Support Mobile Development ##

Install Xcode, launch it, and click 'yes' when asked if you want to install additional components

## Getting Your Certificates and Provisioning Profiles Setup ##

Use [motion-provisioning](https://github.com/HipByte/motion-provisioning)
Add this line to your application's Gemfile:


~~~
gem 'motion-provisioning'
~~~

and then add the info from the motion-provisioning README to your rakefile:


~~~
Motion::Project::App.setup do |app|
  app.name = 'My App'
  app.identifier = 'com.example.myapp'

  app.development do
    app.codesign_certificate = MotionProvisioning.certificate(
      type: :development,
      platform: :ios)

    app.provisioning_profile = MotionProvisioning.profile(
      bundle_identifier: app.identifier,
      app_name: app.name,
      platform: :ios,
      type: :development)
  end

  app.release do
    app.codesign_certificate = MotionProvisioning.certificate(
      type: :distribution,
      platform: :ios)

    app.provisioning_profile = MotionProvisioning.profile(
      bundle_identifier: app.identifier,
      app_name: app.name,
      platform: :ios,
      type: :distribution)
  end
end
~~~

## Running Hello World on the Simulator ##


~~~
motion create hello_world --template=iOS
cd hello_world
bundle install
rake pod:install #optional--only if you add pods to your Rakefile
rake device_name='iPhone 8'
~~~


## Running Hello World on a Device ##

~~~
cd hello_world
rake device
~~~

<span id="tutorial3"></span>
# Adding a `UIButton` Programmatically, and wiring it up to do something when tapped #
~~~
  # viewDidLoad gets called once the view is loaded
  def viewDidLoad
    # call the super method
    super

    # we can change view properties
    self.view.backgroundColor = UIColor.whiteColor

    # Create a button
    # from https://developer.apple.com/documentation/uikit/uibutton/1624028-buttonwithtype
    button = UIButton.buttonWithType(UIButtonTypeSystem)
    button.setTitle("I'm a button", forState:UIControlStateNormal)
    button.sizeToFit
    # This is an arbitrary placement
    button.center = CGPointMake(180, 160)
    # Buttons are views, have have the same properties
    button.backgroundColor = UIColor.lightGrayColor

    # This adds the functionality to the button.
    # Note that the action selector is a string, terminating with a colon
    button.addTarget(self,
                     action: 'button_touched:',
                     forControlEvents: UIControlEventTouchUpInside)

    # Add the button to the view.
    # Note that all UI elements are views, so your button is a subView
    self.view.addSubview(button)
  end

  # This is an example of an action for a UI element
  def button_touched(sender)
    puts "Button was touched."
  end
  ~~~
