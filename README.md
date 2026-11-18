# TranscribeReels audio fixtures

The TranscribeReels project prepared these two comparison datasets on 18 November 2026. The MP3 fixture sizes come from a local FFmpeg run. The sermon worksheet uses authored text and constructed timestamps.

## MP3 prepared-audio sizes

[MP3 prepared-audio CSV](mp3-prepared-size-2026-11-18.csv)

Generate a 440 Hz tone and encode it as a mono MP3 at 16 kHz and 32 kb/s. Decode it to raw signed 16-bit little-endian mono PCM. Run the commands below for 650 seconds, then repeat with 660 seconds and corresponding output filenames.

```sh
ffmpeg -f lavfi -i "sine=frequency=440:sample_rate=16000" -t 650 -ac 1 -c:a libmp3lame -b:a 32k budget-650.mp3
ffmpeg -i budget-650.mp3 -ar 16000 -ac 1 -f s16le budget-650.pcm
```

Measure each file's byte length and hash the PCM file with SHA-256. Add 44 bytes to the PCM length to model the fixture's WAV header. Compare the result with the 20 MiB limit (20,971,520 bytes). Encoder metadata can affect the compressed size. The tones contain no speech, and the data record no recognition results.

| Column | Meaning |
|---|---|
| `requested_seconds` | Requested duration of the synthetic tone in seconds. |
| `encoded_kbps` | Target bitrate of the MP3 encode in kilobits per second. |
| `target_hz` | Target PCM sampling rate in hertz. |
| `channels` | Number of channels in the decoded PCM. |
| `bits_per_sample` | Number of bits per decoded PCM sample. |
| `mp3_bytes` | Measured encoded file length in bytes. |
| `pcm_bytes` | Measured raw decoded file length in bytes. |
| `predicted_wav_bytes` | Measured PCM length plus the 44-byte header, in bytes. |
| `limit_bytes` | Prepared-audio limit used for the comparison, in bytes. |
| `remaining_bytes` | Limit minus predicted WAV size, with a negative value indicating an overage. |
| `pcm_sha256` | SHA-256 hash of the measured raw PCM file. |

## Sermon excerpt timing

[Sermon excerpt timing CSV](sermon-excerpt-clock-2026-11-18.csv)

For each row, add the excerpt start and the cue offset to get the cue's position in the full recording. Compare that result with the answer key. The first two rows locate the same sentence at 1,128 seconds in the full recording. Keep one copy of the sentence and record both excerpt labels. The third cue falls outside excerpt A. The last row intentionally uses an incorrect offset so reviewers can check that their worksheet flags the discrepancy.

| Column | Meaning |
|---|---|
| `excerpt` | Identifier of the source excerpt in the exercise. |
| `excerpt_start_seconds` | Excerpt start position in the full recording, in seconds. |
| `excerpt_end_seconds` | Excerpt end position in the full recording, in seconds. |
| `cue_offset_seconds` | Cue position measured from the excerpt start, in seconds. |
| `source_cue_calculated_seconds` | Sum of the excerpt start and cue offset, in seconds. |
| `source_cue_expected_seconds` | Correct position in the full recording according to the answer key, in seconds. |
| `result` | Whether the row represents a shared cue, unique cue, or timing mismatch. |
| `spoken_reference` | Authored example words associated with the cue. |

## Related pages

- [MP3 to Text](https://transcribereels.com/mp3-to-text)
- [Sermon Transcription](https://transcribereels.com/sermon-transcription)
