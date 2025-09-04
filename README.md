#!/usr/bin/env bash
echo "Initializing AI Reels Project..."

# 1. Create project structure
PROJECT="ai_reels_project"
mkdir -p "$PROJECT"/{personas/viewer}
cd "$PROJECT" || exit

# 2. Create config file
cat > config.yaml <<EOL
personas:
  persona1:
    style: "slime blonde beach editorial, soft lighting, realistic"
  # To add more personas, follow the format above:
  # persona2:
  #   style: "curvy brunette indoor cozy, warm tones"
falai:
  api_key: "YOUR_FALAI_API_KEY"
video:
  duration_seconds: 10
  fps: 30
  width: 720
  height: 1280
EOL

# 3. Script to generate a reel (placeholder, adjust prompt logic)
cat > generate_reel.sh <<'EOL'
#!/usr/bin/env bash
PERSONA="$1"
OUT="personas/$PERSONA"
mkdir -p "$OUT"
# Placeholder: call to Fal.ai reels generation logic
# Save output video to "$OUT/reel.mp4"
echo "Reel generation for $PERSONA would happen here."
EOL

# 4. Script to extract anchor images
cat > extract_anchors.sh <<'EOL'
#!/usr/bin/env bash
PERSONA="$1"
OUT="personas/$PERSONA"
ffmpeg -ss 00:00:05 -i "$OUT/reel.mp4" -frames:v 1 "$OUT/anchor1.png"
ffmpeg -ss 00:00:10 -i "$OUT/reel.mp4" -frames:v 1 "$OUT/anchor2.png"
echo "Anchors saved to $OUT"
EOL

# 5. Script to preview content via web
cat > viewer/run_viewer.sh <<'EOL'
#!/usr/bin/env bash
cd ..
python3 -m http.server 8000
EOL

# 6. Make all scripts executable
chmod +x generate_reel.sh extract_anchors.sh viewer/run_viewer.sh

# 7. Install dependencies
python3 -m pip install --upgrade pip pyyaml requests

echo "Setup complete!"
echo ">> Edit 'config.yaml' and define prompt logic in generate_reel.sh"
echo ">> To test locally:"
echo "cd $(pwd)"
echo "./generate_reel.sh persona1"
echo "./extract_anchors.sh persona1"
echo "cd viewer && ./run_viewer.sh"
echo "Then open http://localhost:8000/ in your browser"
