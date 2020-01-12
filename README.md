A CustomPiOS-based Raspberry Pi image for sharing a webcam.

# Design

The system consists of a Raspberry Pi Zero W running Raspbian configured with CustomPiOS. Components of the system are Linux processes managed by SystemD.

- Raspbian OS
  - HTTPS Proxy
  - Static React client with login, live view, timelapse view, camera settings, and site/admin settings.
  - Node API server. Proxies requests to MJPG-Streamer
  - Simple database (Sqlite or MongoDB) for images and settings. Proxies requests to MJPG-Streamer
  - Octopi running just MJPG-Streamer

# Continuous Integration

The client and server are unit- and integration- tested using Jest, React test tools, and mocks.

The full system is integration-tested using Qemu to boot the image and start the application. Tests are then run against the emulated stack or in the Qemu environment directly against components.

# Continuous Deployment

In a cloud application, the deployment target is an EC2 instance or container and deployment is managed by a service. In IoT, the deployment target is a specific IoT hardware constellation. So for this project, we want to be able to update the server and client running on a Raspberry Pi according to a particular release branch. Using a release branch decouples the purpose and management of the deployment from the particular hardware it runs on.

Deployments:

- autopush: tracks unit- and integration-passing builds of the main development branch. System tests are run against this deployment, and results are used to promote to releases. Promotions can optionally require manual approval.
- [your production deployment]: tracks mainline/fork releases. This is how your hardware setup stays updated. 
- [your development deployment]: tracks a particular branch. You can continuously deploy as you push commits up to your fork. You code against the unit and emulated integration environment.
