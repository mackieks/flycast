# Online microphone request logging

This Windows x64 build uses Release optimization with `ENABLE_LOG=ON`.
It exposes Flycast's Debug settings and preserves the microphone DEBUG logs.
The source revision is recorded in `BUILD_COMMIT.txt` beside the executable.

## Capture a session

1. Extract the artifact and run `flycast.exe`.
2. In Settings > Controls, attach a Microphone to a controller expansion slot.
3. In Settings > Network, enable DCNet. Keep the default emulated ISP credentials.
4. In Settings > Advanced, enable Log to File.
5. In Settings > Debug, set Log Verbosity to Debug. Enable Maple Bus and Peripherals;
   disable the other logging categories to keep the capture manageable.
6. Close Flycast, move aside any existing `flycast.log`, and relaunch it from the
   extracted folder. The log is written in the process's working directory.
7. Run the game through connection, lobby, and voice-chat setup, then exit Flycast
   and save `flycast.log` with the game title and the steps reached.

Look for `maple_microphone`, particularly:

- `Basic_Control DT1`: bit 7 starts/stops sampling; bits 3:2 select the rate;
  bits 1:0 select linear (`00`) or mu-law (`01`) sampling.
- `EXTU_BIT`: the expansion setting requested by the game.
- `set gain`: the requested gain metadata.

Planet Ring is on DCNet's published supported-games list. Other games depend on
their server availability; a failed connection may prevent voice-chat setup.

These logs identify game requests. Flycast currently ignores the Basic_Control
sample-type bits and acknowledges EXTU without implementing its audio expansion,
so emulator audio does not validate MaplePad's mu-law or EXTU output.

DCNet guide: https://github.com/TheArcadeStriker/flycast-wiki/wiki/DCNet

## Download another build

In the fork's Actions tab, open a successful Windows microphone logging run and
download the `flycast-windows-x64-mic-logging` artifact. Pushing a commit to
`codex/mic-logging` triggers this workflow. If GitHub has disabled Actions on the
new fork, enable them first; then push another commit or rerun an available run.
