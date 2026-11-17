# Publishing a new version to Maven Central

The SDK is published to Maven Central under `com.vizbl:ar-sdk`. The source project is not in this repository; this file is for whoever builds the next release.

## Gradle setup in the SDK module

Three things in the library module's `build.gradle.kts` matter for the published POM:

1. **The Compose BOM must be a platform dependency.** Declaring it as a plain `implementation(...)` puts it into the POM as a library and leaves every Compose artifact with `<version>null</version>`, which breaks resolution for consumers. Correct form:

   ```kotlin
   implementation(platform("androidx.compose:compose-bom:2024.05.00"))
   implementation("androidx.compose.ui:ui")
   implementation("androidx.compose.material3:material3")
   ```

   With `platform(...)` Gradle writes concrete versions into the POM.

2. **Coordinates.** `groupId = "com.vizbl"`, `artifactId = "ar-sdk"`, semantic version (`0.3`, `1.0.0`, ...). The old `com.us.vizbl.ar.sdk` group is not ours and cannot be verified on Maven Central.

3. **POM metadata Maven Central requires:** name, description, url, license, developers, scm. Example:

   ```kotlin
   publishing {
       publications {
           create<MavenPublication>("release") {
               groupId = "com.vizbl"
               artifactId = "ar-sdk"
               version = project.version.toString()
               afterEvaluate { from(components["release"]) }
               pom {
                   name.set("Vizbl AR SDK")
                   description.set("Android SDK for Vizbl: place and interact with 3D products in the real world with AR.")
                   url.set("https://github.com/VIZBL/vizbl-android-sdk")
                   licenses { license {
                       name.set("Vizbl Android SDK License (proprietary)")
                       url.set("https://github.com/VIZBL/vizbl-android-sdk/blob/main/LICENSE")
                   } }
                   developers { developer {
                       name.set("Vizbl"); email.set("info@vizbl.com")
                       organization.set("Vizbl Systems, Inc."); organizationUrl.set("https://vizbl.com")
                   } }
                   scm {
                       url.set("https://github.com/VIZBL/vizbl-android-sdk")
                       connection.set("scm:git:https://github.com/VIZBL/vizbl-android-sdk.git")
                   }
               }
           }
       }
   }
   ```

   Also enable `withSourcesJar()` / `withJavadocJar()` on the `android.publishing { singleVariant("release") { ... } }` block, or ship empty `-sources.jar` and `-javadoc.jar`: Maven Central rejects a deployment without them.

## Signing and upload

Maven Central (central.sonatype.com) needs every file signed with a PGP key whose public part is on a key server (keyserver.ubuntu.com), plus `.md5` and `.sha1` checksums. The simplest route is the Gradle plugin `com.vanniktech.maven.publish` with `publishToMavenCentral` and `signAllPublications`; it does the signing, checksums and upload. Credentials come from a Central Portal user token (Account → Generate User Token), stored in `~/.gradle/gradle.properties`, never in the repository.

The `com.vizbl` namespace is registered on central.sonatype.com under the Vizbl Google account. Ask Fedor for access.

## After publishing

1. Update the version in `README.md` here and in `apps/docs/content/docs/android/quick-start.md` in `vizbl/front/space`.
2. Update `vizbl-android-demo` to the new version.
3. Tag this repository with the version.
