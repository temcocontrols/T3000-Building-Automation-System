T3000 Building Automation System 
=================================
This is a hard reboot of our legacy T3000 Building Automation front end, a mature project for managing the air conditioning, lighting, access control and other automation functions of commercial buildings.

HowTo::Build
-------------
With this reboot, we are moving to a CMake based build system. This simplifies the build in many ways:
1. We no longer have to maintain seperate project files for different Visual Studio versions.
2. Our build output does not leave any artifacts in the source (out-of-source builds).
3. CMake based builds are natively supported in Visual Studio 2017 onwards.
4. We should now be able to automate our builds and tests (e.g travis CI).
5. CMake makefiles makes clear the interdependencies in our targets.
6. CMake builds are cross platform supported. (but we do have MFC dependency)
7. ...

The following gives a quick overview of how to create a build. First download the latest CMake https://cmake.org/ tool for windows. Then checkout this repository. Navigate to the folder in the visual studio developer command prompt. Then issue the following commands:
  - mkdir build
  - cd build
  - cmake -G "Visual Studio 14 2015" ..
  - cmake --build . --config Release

Once the build completes, you will find the T3000 executable in build\bin\Release folder.

If you dislike command prompts, please refer to :
![T3000-Building-Automation-System](./CMakeGUISteps.md "CMake GUI guide")



