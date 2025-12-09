# WHIV Radio Stream Recorder

Automated GitHub Actions workflow that records a weekly 1-hour internet radio stream from WHIV.

## What This Does

The workflow automatically:
- Records the WHIV radio stream every Tuesday at 7:00 PM US Central Time
- Captures 1 hour and 1 minute of audio (3660 seconds) to account for any show runover
- Saves the recording as an MP3 file with a timestamp (e.g., `whiv_20241210_1900.mp3`)
- Uploads the recording as a GitHub artifact with 30-day retention
- Logs the file size in the job output for verification

## Stream Details

- **URL:** `http://peridot.streamguys.com:5080/live-mp3`
- **Format:** MP3 (audio copied without re-encoding)
- **Duration:** 3660 seconds (1 hour + 1 minute buffer)

## How to Trigger Manually

1. Go to the **Actions** tab in this repository
2. Select **"Record WHIV Radio Stream"** from the workflows list
3. Click **"Run workflow"** button on the right side
4. Select the branch and click **"Run workflow"**

This is useful for testing or recording outside the regular schedule.

## Where to Find Recordings

1. Navigate to the **Actions** tab
2. Click on a completed workflow run
3. Scroll down to the **Artifacts** section
4. Download the MP3 file (named like `whiv_20241210_1900.mp3`)

Artifacts are retained for **30 days** before automatic deletion.

## Daylight Saving Time Adjustment

The cron schedule is set in UTC and needs manual adjustment for daylight saving time changes:

| Season | US Central Time | UTC Offset | Cron Schedule |
|--------|-----------------|------------|---------------|
| Standard Time (CST) | Nov - Mar | UTC-6 | `0 1 * * 3` (1:00 AM UTC Wed) |
| Daylight Time (CDT) | Mar - Nov | UTC-5 | `0 0 * * 3` (12:00 AM UTC Wed) |

To change the schedule, edit `.github/workflows/record-whiv.yml` and update the cron expression.

## Technical Details

- **Runner:** Ubuntu latest
- **Job timeout:** 70 minutes (to prevent runaway jobs)
- **Recording tool:** ffmpeg with `-c copy` (no re-encoding)
- **Artifact retention:** 30 days
