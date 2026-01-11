# kotlinx-rpc-internship
This is a project for this internship: https://internship.jetbrains.com/projects/1718

Done according to this RFC Style Guide as per task requirements: https://www.rfc-editor.org/rfc/rfc7322

See the final rendered design proposal pdf under jsonrpc_design_proposal_v4.pdf. Everything else are just latex compilation files and a sample implementation closely following the proposal in a zip archive. You can unpack this arhive in the root folder of the current implementation of kotlinx-rpc, update the settings.gradle.kts of the project to look like below and run the tests from the zip to verify everything working.

Updated settings.gradle.kts:
```
/*
 * Copyright 2023-2025 JetBrains s.r.o and contributors. Use of this source code is governed by the Apache 2.0 license.
 */

rootProject.name = "kotlinx-rpc"

enableFeaturePreview("TYPESAFE_PROJECT_ACCESSORS")

pluginManagement {
    includeBuild("gradle-conventions-settings")
    includeBuild("gradle-conventions")

    includeBuild("gradle-plugin")

    apply(from = "gradle-conventions-settings/src/main/kotlin/conventions-repositories.settings.gradle.kts")
}

plugins {
    id("conventions-repositories")
    id("conventions-version-resolution")
    id("conventions-develocity")
    id("krpc-compat-tests")
    id("org.gradle.toolchains.foojay-resolver-convention") version "1.0.0"
}

dependencyResolutionManagement {
    includeBuild("compiler-plugin")
    includeBuild("dokka-plugin")
}

includePublic(":bom")

includePublic(":utils")

includePublic(":core")

// kRPC modules
include(":krpc")
includePublic(":krpc:krpc-core")
includePublic(":krpc:krpc-client")
includePublic(":krpc:krpc-server")
includePublic(":krpc:krpc-logging")
include(":krpc:krpc-test")

include(":krpc:krpc-serialization")
includePublic(":krpc:krpc-serialization:krpc-serialization-core")
includePublic(":krpc:krpc-serialization:krpc-serialization-json")
includePublic(":krpc:krpc-serialization:krpc-serialization-cbor")
includePublic(":krpc:krpc-serialization:krpc-serialization-protobuf")

include(":krpc:krpc-ktor")
includePublic(":krpc:krpc-ktor:krpc-ktor-core")
includePublic(":krpc:krpc-ktor:krpc-ktor-server")
includePublic(":krpc:krpc-ktor:krpc-ktor-client")

// JSON-RPC modules (NEW)
include(":jsonrpc")
includePublic(":jsonrpc:jsonrpc-core")
includePublic(":jsonrpc:jsonrpc-client")
includePublic(":jsonrpc:jsonrpc-server")
includePublic(":jsonrpc:jsonrpc-transport-http")
includePublic(":jsonrpc:jsonrpc-transport-ktor")
includePublic(":jsonrpc:jsonrpc-transport-stdio")
include(":jsonrpc:jsonrpc-codegen")
include(":jsonrpc:jsonrpc-integration-tests")

// Tests
include(":tests")
include(":tests:test-utils")
include(":tests:krpc-compatibility-tests")
include(":tests:krpc-protocol-compatibility-tests")
include(":tests:krpc-protocol-compatibility-tests:test-api")

val kotlinMasterBuild = providers.gradleProperty("kotlinx.rpc.kotlinMasterBuild").orNull == "true"

if (!kotlinMasterBuild) {
    include(":tests:compiler-plugin-tests")
}

include(":jpms-check")
```
Most of the features described in the proposal are implemented in this version, to verify that the solutions described there actually can be implemented with the current state of the Kotlin ecosystem.

The code snippets suggested in the final paper are provided as a sample implementation in accordance to the recent standards and best practices for kotlinx libraries. Feel free to adapt this code for your implementation, like was done in the upper mentioned zip for example.

The proposed RFC has been written with 2 main goals in mind:
1. To implement as many features of the original json-rpc 2.0 protocol as possible
2. To make the final implementation flexible and extendable addition to kotlinx-rpc

You can read more on the additional goals and used approaches in more details in the RFC itself. Enjoy the read!
