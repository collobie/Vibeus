# Vibeus
Vibeus is a free dating app developed to connect our users all around the world without any subscription.
The main motive of vibeus is to help users finding their perfect match without any subscription.


## Plugins used for Vibeus.
```Pluguns Used in Vibeus
  cupertino_icons: ^1.0.0
  firebase_storage: ^3.1.5
  firebase_core: ^0.4.4+3
  firebase_auth: ^0.16.0
  cloud_firestore: ^0.13.5
  flutter_bloc: ^3.2.0
  meta: ^1.1.8
  equatable: ^1.1.1
  rxdart: ^0.23.0
  file_picker: ^1.9.0
  font_awesome_flutter: ^8.8.1
  eva_icons_flutter: ^2.0.0
  extended_image: ^1.5.0
  timeago: 
  uuid: ^2.0.4
  image_picker: ^0.6.7+17
  transparent_image: ^1.0.0
  flutter_datetime_picker: ^1.4.0
  material_design_icons_flutter:
  flutter_linkify: ^4.0.2
  url_launcher: ^5.7.10
  onesignal_flutter: ^2.6.2
  advance_pdf_viewer: ^1.2.1+2
  geolocator: ^5.3.1
```

## OneSignal in vibeus
```
buildscript {
    repositories {
        // ...
        maven { url 'https://plugins.gradle.org/m2/' } // Gradle Plugin Portal
    }
    dependencies {
        // ...
        // OneSignal-Gradle-Plugin
        classpath 'gradle.plugin.com.onesignal:onesignal-gradle-plugin:[0.12.6, 0.99.99]'
    }
}

apply plugin: 'com.onesignal.androidsdk.onesignal-gradle-plugin'
```

## Using Bloc method to authenticaticate Users
```
class AuthenticationBloc
    extends Bloc<AuthenticationEvent, AuthenticationState> {
  final UserRepository _userRepository;

  AuthenticationBloc({@required UserRepository userRepository})
      : assert(userRepository != null),
        _userRepository = userRepository;

  @override
  AuthenticationState get initialState => Uninitialized();

  @override
  Stream<AuthenticationState> mapEventToState(
    AuthenticationEvent event,
  ) async* {
    if (event is AppStarted) {
      yield* _mapAppStartedToState();
    } else if (event is LoggedIn) {
      yield* _mapLoggedInToState();
    } else if (event is LoggedOut) {
      yield* _mapLoggedOutToState();
    }
  }

  Stream<AuthenticationState> _mapAppStartedToState() async* {
    try {
      final isSignedIn = await _userRepository.isSignedIn();
      if (isSignedIn) {
        final uid = await _userRepository.getUser();
        final isFirstTime = await _userRepository.isFirstTime(uid);

        if (!isFirstTime) {
          yield AuthenticatedButNotSet(uid);
        } else {
          yield Authenticated(uid);
        }
      } else {
        yield Unauthenticated();
      }
    } catch (_) {
      yield Unauthenticated();
    }
  }

  Stream<AuthenticationState> _mapLoggedInToState() async* {
    final isFirstTime =
        await _userRepository.isFirstTime(await _userRepository.getUser());

    if (!isFirstTime) {
      yield AuthenticatedButNotSet(await _userRepository.getUser());
    } else {
      yield Authenticated(await _userRepository.getUser());
    }
  }

  Stream<AuthenticationState> _mapLoggedOutToState() async* {
    yield Unauthenticated();
    _userRepository.signOut();
  }
}
```

## License
```
MIT License

Copyright (c) 2021 Vibeus

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```
## Getting Started


A few resources to get you started if this is your first Flutter project:

- [Flutter](https://flutter.dev).
- [FlutterFire](https://firebase.flutter.dev/)

For help getting started with Flutter, view flutter ducmention
[online documentation](https://flutter.dev/dcs), which offers tutorials,
samples, guidance on mobile development, and a full API reference.

## [Vibeus Privacy Policy](https://github.com/vibeus-con/vibeusprivacy/blob/main/Privacy.md)


