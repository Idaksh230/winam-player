# winam-player<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Winam Music Player</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }

        body {
            background: linear-gradient(135deg, #1a2a6c, #b21f1f, #001510);
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            padding: 20px;
        }

        .player-container {
            width: 100%;
            max-width: 420px;
            background: rgba(15, 15, 35, 0.85);
            border-radius: 20px;
            box-shadow: 0 15px 30px rgba(0, 0, 0, 0.4);
            overflow: hidden;
            backdrop-filter: blur(10px);
            border: 1px solid rgba(255, 255, 255, 0.1);
        }

        .header {
            background: rgba(10, 10, 25, 0.9);
            padding: 20px;
            text-align: center;
            border-bottom: 1px solid rgba(255, 255, 255, 0.1);
        }

        .logo {
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 10px;
            color: #fff;
        }

        .logo i {
            font-size: 28px;
            color: #4cc9f0;
        }

        .logo h1 {
            font-size: 28px;
            font-weight: 700;
            letter-spacing: 1px;
        }

        .logo span {
            color: #4cc9f0;
        }

        .album-art {
            width: 220px;
            height: 220px;
            margin: 30px auto 20px;
            border-radius: 50%;
            background: linear-gradient(45deg, #ff9a9e, #fad0c4);
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.3);
            overflow: hidden;
            border: 5px solid rgba(255, 255, 255, 0.1);
            display: flex;
            align-items: center;
            justify-content: center;
            animation: rotate 20s linear infinite;
            animation-play-state: paused;
        }

        .album-art.playing {
            animation-play-state: running;
        }

        .album-art img {
            width: 100%;
            height: 100%;
            object-fit: cover;
        }

        .album-art .placeholder {
            font-size: 48px;
            color: rgba(255, 255, 255, 0.7);
        }

        .song-info {
            text-align: center;
            padding: 0 20px;
            margin-bottom: 25px;
        }

        .song-info h2 {
            color: white;
            font-size: 24px;
            margin-bottom: 8px;
            white-space: nowrap;
            overflow: hidden;
            text-overflow: ellipsis;
        }

        .song-info p {
            color: rgba(255, 255, 255, 0.7);
            font-size: 16px;
        }

        .progress-container {
            padding: 0 30px;
            margin-bottom: 25px;
        }

        .progress-bar {
            width: 100%;
            height: 6px;
            background: rgba(255, 255, 255, 0.1);
            border-radius: 3px;
            cursor: pointer;
            position: relative;
            margin-bottom: 5px;
        }

        .progress {
            height: 100%;
            background: linear-gradient(to right, #4cc9f0, #4361ee);
            border-radius: 3px;
            width: 0%;
            transition: width 0.1s linear;
        }

        .time {
            display: flex;
            justify-content: space-between;
            color: rgba(255, 255, 255, 0.7);
            font-size: 14px;
        }

        .controls {
            display: flex;
            justify-content: center;
            align-items: center;
            gap: 25px;
            padding: 0 30px 30px;
        }

        .control-btn {
            background: none;
            border: none;
            color: white;
            cursor: pointer;
            width: 50px;
            height: 50px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            transition: all 0.3s ease;
        }

        .control-btn:hover {
            background: rgba(255, 255, 255, 0.1);
            transform: scale(1.05);
        }

        .control-btn:active {
            transform: scale(0.95);
        }

        .control-btn.play-pause {
            width: 70px;
            height: 70px;
            background: linear-gradient(135deg, #4361ee, #4cc9f0);
            box-shadow: 0 5px 15px rgba(67, 97, 238, 0.4);
        }

        .control-btn.play-pause:hover {
            background: linear-gradient(135deg, #4cc9f0, #4361ee);
            transform: scale(1.08);
        }

        .volume-container {
            display: flex;
            align-items: center;
            gap: 10px;
            padding: 0 30px 25px;
        }

        .volume-container i {
            color: rgba(255, 255, 255, 0.7);
            font-size: 18px;
        }

        .volume-slider {
            -webkit-appearance: none;
            width: 100%;
            height: 5px;
            background: rgba(255, 255, 255, 0.1);
            border-radius: 5px;
            outline: none;
        }

        .volume-slider::-webkit-slider-thumb {
            -webkit-appearance: none;
            width: 16px;
            height: 16px;
            border-radius: 50%;
            background: #4cc9f0;
            cursor: pointer;
        }

        .playlist {
            padding: 20px;
            background: rgba(10, 10, 25, 0.6);
            border-top: 1px solid rgba(255, 255, 255, 0.1);
            max-height: 250px;
            overflow-y: auto;
        }

        .playlist h3 {
            color: rgba(255, 255, 255, 0.9);
            margin-bottom: 15px;
            font-size: 18px;
            padding-left: 10px;
        }

        .playlist-item {
            display: flex;
            align-items: center;
            padding: 12px 15px;
            border-radius: 10px;
            margin-bottom: 10px;
            cursor: pointer;
            transition: background 0.3s;
        }

        .playlist-item:hover {
            background: rgba(255, 255, 255, 0.08);
        }

        .playlist-item.playing {
            background: rgba(76, 201, 240, 0.15);
            border-left: 3px solid #4cc9f0;
        }

        .playlist-item img {
            width: 45px;
            height: 45px;
            border-radius: 8px;
            margin-right: 15px;
            object-fit: cover;
        }

        .playlist-item .song-details {
            flex: 1;
        }

        .playlist-item .song-details h4 {
            color: white;
            font-size: 16px;
            margin-bottom: 5px;
        }

        .playlist-item .song-details p {
            color: rgba(255, 255, 255, 0.6);
            font-size: 14px;
        }

        .playlist-item .duration {
            color: rgba(255, 255, 255, 0.6);
            font-size: 14px;
            margin-left: 10px;
        }

        @keyframes rotate {
            from { transform: rotate(0deg); }
            to { transform: rotate(360deg); }
        }

        /* Scrollbar styling */
        .playlist::-webkit-scrollbar {
            width: 8px;
        }

        .playlist::-webkit-scrollbar-track {
            background: rgba(0, 0, 0, 0.1);
            border-radius: 4px;
        }

        .playlist::-webkit-scrollbar-thumb {
            background: rgba(76, 201, 240, 0.5);
            border-radius: 4px;
        }

        .playlist::-webkit-scrollbar-thumb:hover {
            background: #4cc9f0;
        }
    </style>
</head>
<body>
    <div class="player-container">
        <div class="header">
            <div class="logo">
                <i class="fas fa-music"></i>
                <h1>WIN<span>AM</span></h1>
            </div>
        </div>
        
        <div class="album-art" id="albumArt">
            <div class="placeholder">
                <i class="fas fa-music"></i>
            </div>
        </div>
        
        <div class="song-info">
            <h2 id="songTitle">Melodic Journey</h2>
            <p id="songArtist">Harmony Masters</p>
        </div>
        
        <div class="progress-container">
            <div class="progress-bar" id="progressBar">
                <div class="progress" id="progress"></div>
            </div>
            <div class="time">
                <span id="currentTime">0:00</span>
                <span id="duration">3:45</span>
            </div>
        </div>
        
        <div class="controls">
            <button class="control-btn" id="prevBtn">
                <i class="fas fa-step-backward"></i>
            </button>
            <button class="control-btn play-pause" id="playBtn">
                <i class="fas fa-play"></i>
            </button>
            <button class="control-btn" id="nextBtn">
                <i class="fas fa-step-forward"></i>
            </button>
        </div>
        
        <div class="volume-container">
            <i class="fas fa-volume-down"></i>
            <input type="range" class="volume-slider" id="volumeSlider" min="0" max="1" step="0.01" value="0.7">
            <i class="fas fa-volume-up"></i>
        </div>
        
        <div class="playlist">
            <h3>Playlist</h3>
            <div class="playlist-item playing">
                <img src="https://images.unsplash.com/photo-1470225620780-dba8ba36b745?ixlib=rb-1.2.1&auto=format&fit=crop&w=200&q=80" alt="Album Cover">
                <div class="song-details">
                    <h4>Melodic Journey</h4>
                    <p>Harmony Masters</p>
                </div>
                <div class="duration">3:45</div>
            </div>
            <div class="playlist-item">
                <img src="https://images.unsplash.com/photo-1494232410401-ad00d5433cfa?ixlib=rb-1.2.1&auto=format&fit=crop&w=200&q=80" alt="Album Cover">
                <div class="song-details">
                    <h4>Midnight Drive</h4>
                    <p>Synthwave Collective</p>
                </div>
                <div class="duration">4:20</div>
            </div>
            <div class="playlist-item">
                <img src="https://images.unsplash.com/photo-1458560871784-56d23406c091?ixlib=rb-1.2.1&auto=format&fit=crop&w=200&q=80" alt="Album Cover">
                <div class="song-details">
                    <h4>Ocean Waves</h4>
                    <p>Nature Sounds</p>
                </div>
                <div class="duration">5:15</div>
            </div>
            <div class="playlist-item">
                <img src="https://images.unsplash.com/photo-1507838153414-b4b713384a76?ixlib=rb-1.2.1&auto=format&fit=crop&w=200&q=80" alt="Album Cover">
                <div class="song-details">
                    <h4>Electric Dreams</h4>
                    <p>Neon Future</p>
                </div>
                <div class="duration">3:30</div>
            </div>
            <div class="playlist-item">
                <img src="https://images.unsplash.com/photo-1470225620780-dba8ba36b745?ixlib=rb-1.2.1&auto=format&fit=crop&w=200&q=80" alt="Album Cover">
                <div class="song-details">
                    <h4>Summer Vibes</h4>
                    <p>Beach House</p>
                </div>
                <div class="duration">4:05</div>
            </div>
        </div>
    </div>

    <audio id="audioPlayer">
        <!-- We'll dynamically load audio sources -->
    </audio>

    <script>
        // DOM Elements
        const audioPlayer = document.getElementById('audioPlayer');
        const playBtn = document.getElementById('playBtn');
        const prevBtn = document.getElementById('prevBtn');
        const nextBtn = document.getElementById('nextBtn');
        const progressBar = document.getElementById('progressBar');
        const progress = document.getElementById('progress');
        const currentTimeEl = document.getElementById('currentTime');
        const durationEl = document.getElementById('duration');
        const volumeSlider = document.getElementById('volumeSlider');
        const albumArt = document.getElementById('albumArt');
        const songTitle = document.getElementById('songTitle');
        const songArtist = document.getElementById('songArtist');
        const playlistItems = document.querySelectorAll('.playlist-item');
        
        // Sample playlist data
        const playlist = [
            {
                title: "Melodic Journey",
                artist: "Harmony Masters",
                cover: "https://images.unsplash.com/photo-1470225620780-dba8ba36b745?ixlib=rb-1.2.1&auto=format&fit=crop&w=200&q=80",
                duration: "3:45",
                file: "https://www.soundhelix.com/examples/mp3/SoundHelix-Song-1.mp3"
            },
            {
                title: "Midnight Drive",
                artist: "Synthwave Collective",
                cover: "https://images.unsplash.com/photo-1494232410401-ad00d5433cfa?ixlib=rb-1.2.1&auto=format&fit=crop&w=200&q=80",
                duration: "4:20",
                file: "https://www.soundhelix.com/examples/mp3/SoundHelix-Song-2.mp3"
            },
            {
                title: "Ocean Waves",
                artist: "Nature Sounds",
                cover: "https://images.unsplash.com/photo-1458560871784-56d23406c091?ixlib=rb-1.2.1&auto=format&fit=crop&w=200&q=80",
                duration: "5:15",
                file: "https://www.soundhelix.com/examples/mp3/SoundHelix-Song-3.mp3"
            },
            {
                title: "Electric Dreams",
                artist: "Neon Future",
                cover: "https://images.unsplash.com/photo-1507838153414-b4b713384a76?ixlib=rb-1.2.1&auto=format&fit=crop&w=200&q=80",
                duration: "3:30",
                file: "https://www.soundhelix.com/examples/mp3/SoundHelix-Song-4.mp3"
            },
            {
                title: "Summer Vibes",
                artist: "Beach House",
                cover: "https://images.unsplash.com/photo-1470225620780-dba8ba36b745?ixlib=rb-1.2.1&auto=format&fit=crop&w=200&q=80",
                duration: "4:05",
                file: "https://www.soundhelix.com/examples/mp3/SoundHelix-Song-5.mp3"
            }
        ];
        
        let currentTrack = 0;
        
        // Load track
        function loadTrack(index) {
            const track = playlist[index];
            
            // Update UI
            songTitle.textContent = track.title;
            songArtist.textContent = track.artist;
            albumArt.innerHTML = `<img src="${track.cover}" alt="${track.title}">`;
            durationEl.textContent = track.duration;
            
            // Set audio source
            audioPlayer.src = track.file;
            audioPlayer.load();
            
            // Update playlist highlights
            playlistItems.forEach((item, i) => {
                if (i === index) {
                    item.classList.add('playing');
                } else {
                    item.classList.remove('playing');
                }
            });
            
            // Play the track
            playTrack();
        }
        
        // Play track
        function playTrack() {
            audioPlayer.play();
            playBtn.innerHTML = '<i class="fas fa-pause"></i>';
            albumArt.classList.add('playing');
        }
        
        // Pause track
        function pauseTrack() {
            audioPlayer.pause();
            playBtn.innerHTML = '<i class="fas fa-play"></i>';
            albumArt.classList.remove('playing');
        }
        
        // Next track
        function nextTrack() {
            currentTrack++;
            if (currentTrack >= playlist.length) {
                currentTrack = 0;
            }
            loadTrack(currentTrack);
        }
        
        // Previous track
        function prevTrack() {
            currentTrack--;
            if (currentTrack < 0) {
                currentTrack = playlist.length - 1;
            }
            loadTrack(currentTrack);
        }
        
        // Update progress bar
        function updateProgress(e) {
            const { duration, currentTime } = e.srcElement;
            const progressPercent = (currentTime / duration) * 100;
            progress.style.width = `${progressPercent}%`;
            
            // Update time display
            const currentMinutes = Math.floor(currentTime / 60);
            const currentSeconds = Math.floor(currentTime % 60);
            currentTimeEl.textContent = `${currentMinutes}:${currentSeconds < 10 ? '0' : ''}${currentSeconds}`;
        }
        
        // Set progress bar
        function setProgress(e) {
            const width = this.clientWidth;
            const clickX = e.offsetX;
            const duration = audioPlayer.duration;
            
            audioPlayer.currentTime = (clickX / width) * duration;
        }
        
        // Set volume
        function setVolume() {
            audioPlayer.volume = volumeSlider.value;
        }
        
        // Event Listeners
        playBtn.addEventListener('click', () => {
            const isPlaying = albumArt.classList.contains('playing');
            if (isPlaying) {
                pauseTrack();
            } else {
                playTrack();
            }
        });
        
        prevBtn.addEventListener('click', prevTrack);
        nextBtn.addEventListener('click', nextTrack);
        
        audioPlayer.addEventListener('timeupdate', updateProgress);
        audioPlayer.addEventListener('ended', nextTrack);
        
        progressBar.addEventListener('click', setProgress);
        
        volumeSlider.addEventListener('input', setVolume);
        
        // Playlist item click
        playlistItems.forEach((item, index) => {
            item.addEventListener('click', () => {
                currentTrack = index;
                loadTrack(currentTrack);
            });
        });
        
        // Initialize player with first track
        loadTrack(currentTrack);
    </script>
</body>
</html>
