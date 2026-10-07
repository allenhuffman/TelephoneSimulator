System recordings (all optional; synthesized tones are used if missing):
  dialtone.mp3 - looped when the receiver is picked up
  reorder.mp3  - looped when the dial tone times out
  invalid.mp3  - played when the dialed number is not in the CSV or has no audio file
  busy.mp3     - played when a phone calls its own number

Shared number recordings go in ../ named "<number>.mp3" matching the number in numbers.csv (e.g. ../030-263.mp3).
Optional phone-specific recordings go in ../<phone-number-without-dashes>/ named "<number>.mp3" (e.g. ../314112/8000.mp3). They are checked before the shared ones.
