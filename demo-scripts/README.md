# Demonstration Scripts

## Purpose
This directory contains scripts and instructions for demonstrating specific capabilities of the EcoRoute Advisor system, particularly highlighting the latency advantages of AWS Wavelength Zones under constrained network conditions.

## User Story
As a judge (or evaluator), I want a demo script that throttles bandwidth to 1 Mbps and shows Wavelength still hitting < 1 s replans so that the telco value prop is obvious.

## Acceptance Criteria
-   **AC-1:** The script outputs side-by-side latency numbers (Wavelength vs. Region) in the terminal.
-   **AC-2:** This README explains how to run the script on macOS and Windows.

## Script Details (`latency_demo.sh` or similar)
-   **Functionality:**
    -   The script will likely make concurrent or sequential API calls to both the Wavelength-deployed `RoutePlanner` endpoint and the Region-deployed `RoutePlanner` endpoint.
    -   It will measure and record the response time for each call.
    -   It will output these latency figures clearly in the terminal, allowing for easy comparison.
    -   The script itself might not perform bandwidth throttling but will rely on external tools. The instructions below will cover how to set up throttling.
-   **Parameters:**
    -   May accept API endpoint URLs as parameters or read them from a config file.
    -   Could take a number of iterations to run the test multiple times.

## Bandwidth Throttling Instructions

### macOS
macOS has built-in tools like `pf` (Packet Filter) or the Network Link Conditioner (part of Additional Tools for Xcode) that can be used to simulate various network conditions, including bandwidth throttling.

**Using Network Link Conditioner (Recommended for ease of use):**
1.  **Install Additional Tools for Xcode:**
    -   Download from the Apple Developer website (search for "Additional Tools for Xcode").
    -   Mount the DMG and find "Network Link Conditioner.prefPane" in the "Hardware" folder. Double-click to install it in System Preferences.
2.  **Configure Throttling:**
    -   Open System Preferences and go to "Network Link Conditioner".
    -   Enable it.
    -   Choose a profile that matches 1 Mbps or create a custom profile with:
        -   Downlink Bandwidth: 1 Mbps
        -   Uplink Bandwidth: (Adjust as needed, e.g., 1 Mbps or lower)
        -   Packet Loss, Delay: (Keep at 0 or minimal for this specific test, unless testing other conditions)
3.  **Run the Demo Script:** Execute the `latency_demo.sh` script in your terminal while the Network Link Conditioner is active.
4.  **Disable Throttling:** Turn off the Network Link Conditioner in System Preferences after the demo.

**Using `pf` (More advanced):**
(Detailed instructions for `pf` can be complex and will be added if this method is preferred. It typically involves creating rules in `/etc/pf.conf` and using `dnctl` to create pipes for traffic shaping.)

### Windows
Windows can use tools like `Windows Sandbox` with network limitations, third-party software like `NetLimiter` or `TMeter`, or built-in `QoS Policies`.

**Using Group Policy for QoS (Built-in, for outbound traffic):**
1.  **Open Group Policy Editor:** Run `gpedit.msc`.
2.  **Navigate to Policy-based QoS:**
    -   Computer Configuration > Windows Settings > Policy-based QoS.
3.  **Create a New Policy:**
    -   Right-click and select "Create new policy...".
    -   Policy Name: e.g., "Throttle to 1Mbps".
    -   Specify Outbound Throttle Rate: Check this box and enter `1` MBps.
    -   Click Next. Apply to "All applications" or specify the application/browser used for testing.
    -   Click Next. Apply to "Any source IP address" and "Any destination IP address".
    -   Click Next. Apply to "TCP and UDP" (or as needed).
    -   Click Finish.
4.  **Run the Demo Script:** Execute the demo script.
5.  **Remove Policy:** Delete the QoS policy after the demo to restore normal bandwidth.

**Using a Third-Party Tool (e.g., NetLimiter - may require a license):**
1.  Install NetLimiter or a similar tool.
2.  Configure a rule to limit the bandwidth for the specific application running the demo script (e.g., your terminal or browser) or globally to 1 Mbps.
3.  Run the demo script.
4.  Disable or remove the rule.

## Running the Script
```bash
# Example (actual command might vary)
./latency_demo.sh --wavelength-url <URL1> --region-url <URL2>
```
The script will then output latency readings for each endpoint.
