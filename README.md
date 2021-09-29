# Telemetry Overview

An open specification for recording and measuring statistical data from real-world use of digital apps & platforms

## [Understanding Telemetry](understand.md)

## [Telemetry Specifications](specification.md)

## [Sending Telemetry](sending_telemetry.md)

## [Consuming Telemetry](consuming_telemetry.md)

## Library References

* Standalone JS Library

  The standalone telemetry JS library allows users to capture telemetry data without the restrictions of using any app that uses the Genie SDK, the EkStep content player or the EkStep or Sunbird portal. Partner users can use the JS library to log and sync telemetry data. They can decide how to use the Telemetry JS library, and integrate it with their app, webpage or web service

  [Standalone JS Library](jslibrary.md) section will help you in understanding better how JS library serves the purpose of capturing telemetry data

* HTML Interface Library

  The ContentRenderer handles telemetry events for ECML content. HTML content has functionality such as click, navigation, assessment, etc. These functionalities are specific to or different for individual HTML content pieces.

  [HTML Interface Library](html_interface_library.md) details information about the library used to log telemetry events for HTML content.

* AuthToken Generator JS Library

  The AuthToken generator JS Library is used to generate or refresh the user AuthToken. The Authtoken is mandatory for any API request.

  [AuthToken Generator JS Library](authtokengenerator_jslibrary.md) details method and process of generating key and tokens.

