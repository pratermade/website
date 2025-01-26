+++
date = '2025-01-26T18:58:48Z'
draft = false
cover = 'images/cover.webp'
title = 'Cloudless Bambu 3dprinter'
thumbnail = 'images/Thumb.webp'
+++


# Securing and Enhancing My 3D Printer with Home Assistant

This weekend, I undertook a project to make my 3D printing setup more secure and user-friendly. Specifically, I removed my Bambu A1 printer from public internet access, integrated it with Home Assistant, added a Wyze camera for monitoring, and enabled remote access using Tailscale. Here’s a step-by-step breakdown of what I did and why.

## Why Take the Printer Offline?

Recently, Bambu Labs announced changes to their firmware authentication process, raising concerns in the community. While the changes aimed to address security issues, they also posed risks of breaking third-party integrations that many users rely on. Although Bambu Labs later clarified they wouldn’t eliminate local LAN functionality, I decided to proactively safeguard my setup by disabling the printer’s cloud connectivity.

### Steps to Disable Cloud Connectivity
1. **Printer Settings**: Disabled cloud connectivity within the printer’s settings menu.
2. **Router Configuration**: Blocked the printer from accessing the internet by denying its connection at the router level. This ensures no unwanted firmware updates can override my current functionality.

## Integrating the Printer with Home Assistant

To regain remote access without relying on the cloud, I integrated the printer with my existing Home Assistant setup running on a Raspberry Pi 4. This allowed me to monitor and control the printer locally. Here’s how I did it:

1. **Installed the Bambu Labs Add-On**: Using Home Assistant’s add-on store, I installed a third-party add-on called [HACS](https://hacs.xyz/).
2. **Added Controls**: Configured the integration to allow essential actions like pausing or stopping a print if something goes wrong.

Home Assistant provides various sensors and controls, but for my setup, I focused on the basics to ensure smooth operation.

## Adding a Wyze Camera for Better Monitoring

The built-in camera on the Bambu A1 printer didn’t meet my needs for real-time monitoring. It struggled in low light and only captured snapshots intermittently. To improve this:

1. **Repurposed a Wyze Camera**: I attached a Wyze Cam v2 to the printer’s crossbar using a wire tie, positioning it to capture the print bed clearly.
2. **Enabled Local Access**: Used the Wyze Camera Home Assistant add-on to integrate the camera feed into my Home Assistant setup.


{{< lightbox group="Wyze Camera" >}}
    images/wyzecam.jpg
{{< /lightbox >}}

### How to Set Up the Wyze Camera with the Home Assistant Add-On
1. Open Home Assistant and navigate to the Add-on Store.
2. Search for and install the "Wyze Bridge" add-on.
3. Configure the add-on by providing your Wyze account credentials:
   ```yaml
   wyze_email: "your_email"
   wyze_password: "your_password"
   ```
4. Start the add-on and note the RTSP URL for your camera.
5. Add the RTSP stream to your Home Assistant configuration to display the feed on your dashboard.

{{< lightbox group="Dashboard" >}}
    images/dashboard.png
{{< /lightbox >}}

## Creating a Custom Dashboard in Home Assistant

I designed a dedicated dashboard in Home Assistant to monitor my printer. It includes:
- Real-time video feeds from both the Bambu camera and Wyze Cam.
- Current print job details (file name, progress, layers, etc.).
- Printer bed and nozzle temperature readings.
- Control buttons for pausing, playing, and stopping prints.

## Enabling Remote Access with Tailscale

To access my setup remotely, I integrated Home Assistant with Tailscale, a VPN service. Tailscale provides secure, peer-to-peer connections between devices, eliminating the need for complex network configurations.

### Setting Up Tailscale
1. Install the Tailscale add-on in Home Assistant.
2. Link it to your Tailscale account.
3. Enable subnet routing to access all resources on your network.

Now, I can monitor and control my printer from anywhere using the Home Assistant mobile app.

## Adding a Safety Net: Shelly Plug US

As an extra precaution, I connected my printer to a Shelly Plug US, which integrates with Home Assistant. This smart plug allows me to cut power to the printer remotely if something goes wrong and I’m not at home.

## Conclusion

With these enhancements, my 3D printer setup is now more secure, functional, and accessible. Disabling cloud connectivity protects my investment from unwanted updates, while Home Assistant and Tailscale provide seamless control and monitoring. The added Wyze camera and Shelly Plug ensure I have full oversight and control, whether I’m at home or on the go.

If you’re concerned about the future of your connected devices, I highly recommend taking similar steps to safeguard and optimize your setup.

