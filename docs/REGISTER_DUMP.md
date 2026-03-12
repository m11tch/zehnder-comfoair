# Register Dump

An optional component that reads all holding registers (0–374) from the ComfoAir unit and makes them available as a **CSV download** via the ESP device's internal web server.

This is useful for gathering register data from different ComfoAir models without cluttering Home Assistant with hundreds of sensors.

## Setup

Add `components/register_dump.yaml` to your packages:

```yaml
packages:
  remote_package:
    url: https://github.com/CodedCactus/zehnder-comfoair
    ref: main
    files: [components/zehnder.yaml,
            components/fan.yaml,
            components/register_dump.yaml]  # register dump
    refresh: 0s
```

## Usage

1. Navigate directly to the register dump page:
   `http://<device-ip>/registers`
2. Use the top navbar to switch between the **Dashboard** and **Register Dump**.
3. Click **"Start New Dump"**. The page will show the progress and update automatically (no refresh required).
4. Once the status shows **"Complete"**, a **"Download registers.csv"** button will appear.
5. Click the button to download the full CSV file.

## Technical details

- **Endpoints**:
    - `/registers`: Control UI and status
    - `/registers/status`: JSON status for polling
    - `/registers/download.csv`: CSV data export
- **Modbus**: Registers are read in batches of 50 (8 batches total).
- **Memory**: The result is stored in RAM as a CSV string (~5-10 KB). A new dump will overwrite previous data.
- **Requirements**: This component automatically enables the `web_server` component.
