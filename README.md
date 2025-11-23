<html lang="fa" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>پخش کننده موسیقی</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }

        body {
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            background-color: #000;
            color: white;
            overflow: hidden;
            position: relative;
        }

        .background-gif {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            object-fit: cover;
            z-index: 1;
            opacity: 0;
            transition: opacity 1s ease;
        }

        .background-gif.show {
            opacity: 0.7;
        }

        .container {
            text-align: center;
            z-index: 10;
            position: relative;
            background: rgba(0, 0, 0, 0.7);
            padding: 30px;
            border-radius: 20px;
            backdrop-filter: blur(10px);
            border: 1px solid rgba(255, 255, 255, 0.1);
            max-width: 90%;
            width: 400px;
        }

        .play-button {
            width: 120px;
            height: 120px;
            background: linear-gradient(135deg, #6a11cb 0%, #2575fc 100%);
            border-radius: 50%;
            display: flex;
            justify-content: center;
            align-items: center;
            cursor: pointer;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.5);
            transition: all 0.4s ease;
            margin: 0 auto 15px;
            position: relative;
            overflow: hidden;
        }

        .play-button::before {
            content: '';
            position: absolute;
            width: 150%;
            height: 150%;
            background: rgba(255, 255, 255, 0.1);
            border-radius: 50%;
            transform: scale(0);
            transition: transform 0.5s ease;
        }

        .play-button:hover::before {
            transform: scale(1);
        }

        .play-button:hover {
            transform: scale(1.1);
            box-shadow: 0 15px 40px rgba(106, 17, 203, 0.7);
        }

        .play-icon {
            font-size: 40px;
            color: white;
            z-index: 2;
        }

        .click-text {
            color: #a0a0c0;
            font-size: 16px;
            margin-bottom: 10px;
            font-style: italic;
        }

        .hidden {
            display: none !important;
        }

        .audio-player {
            margin-top: 20px;
            animation: fadeIn 0.8s ease;
        }

        .track-info {
            margin-bottom: 20px;
        }

        .track-title {
            font-size: 22px;
            margin-bottom: 5px;
            font-weight: 600;
        }

        .creator-link {
            display: inline-block;
            margin-top: 25px;
            padding: 12px 25px;
            background: linear-gradient(135deg, #0088cc, #34b7f1);
            color: white;
            text-decoration: none;
            border-radius: 50px;
            font-weight: 600;
            transition: all 0.3s ease;
            box-shadow: 0 5px 15px rgba(0, 136, 204, 0.4);
        }

        .creator-link:hover {
            transform: translateY(-3px);
            box-shadow: 0 8px 20px rgba(0, 136, 204, 0.6);
        }

        .creator-link i {
            margin-left: 8px;
        }

        .progress-area {
            margin-top: 20px;
        }

        .progress-bar {
            height: 6px;
            width: 100%;
            background: rgba(255, 255, 255, 0.1);
            border-radius: 10px;
            cursor: pointer;
            margin-bottom: 8px;
        }

        .progress {
            height: 100%;
            width: 0%;
            background: linear-gradient(to right, #6a11cb, #2575fc);
            border-radius: 10px;
            position: relative;
            transition: width 0.1s linear;
        }

        .time {
            display: flex;
            justify-content: space-between;
            font-size: 14px;
            color: #a0a0c0;
        }

        .loading {
            color: #a0a0c0;
            margin-top: 10px;
            font-size: 14px;
        }

        .auto-play-info {
            color: #4CAF50;
            margin-top: 10px;
            font-size: 12px;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 5px;
        }

        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(20px); }
            to { opacity: 1; transform: translateY(0); }
        }

        .pulse {
            animation: pulse 2s infinite;
        }

        @keyframes pulse {
            0% { box-shadow: 0 0 0 0 rgba(106, 17, 203, 0.7); }
            70% { box-shadow: 0 0 0 15px rgba(106, 17, 203, 0); }
            100% { box-shadow: 0 0 0 0 rgba(106, 17, 203, 0); }
        }
    </style>
</head>
<body>
    <img src="https://uploadkon.ir/uploads/c45b23_25InShot-20251123-150816028.gif" class="background-gif" id="backgroundGif" alt="پس‌زمینه متحرک">
    
    <div class="container">
        <div class="play-button pulse" id="playButton">
            <i class="fas fa-play play-icon"></i>
        </div>
        
        <div class="click-text">کلیک کن بچه :)</div>
        
        <div class="audio-player hidden" id="audioPlayer">
            <div class="track-info">
                <div class="track-title">موسیقی انتخابی</div>
                <div class="auto-play-info">
                    <i class="fas fa-redo"></i>
                    پخش خودکار فعال
                </div>
            </div>
            
            <div class="progress-area">
                <div class="progress-bar" id="progressBar">
                    <div class="progress" id="progress"></div>
                </div>
                <div class="time">
                    <span id="currentTime">0:00</span>
                    <span id="duration">0:00</span>
                </div>
            </div>
            
            <a href="https://t.me/u0v0n" class="creator-link" target="_blank" onclick="sendTelegramMessage()">
                <i class="fab fa-telegram"></i> سازنده
            </a>
        </div>

        <div class="loading hidden" id="loadingMessage">
            در حال بارگذاری موسیقی...
        </div>
    </div>

    <audio id="audioElement" preload="auto" loop>
        <source src="https://uploadkon.ir/uploads/114523_25mardsayeh-14040902-144859428-online-audio-converter-com-1-.mp3" type="audio/mpeg">
        مرورگر شما از پخش کننده صوتی پشتیبانی نمی‌کند.
    </audio>

    <script>
        const playButton = document.getElementById('playButton');
        const audioElement = document.getElementById('audioElement');
        const audioPlayer = document.getElementById('audioPlayer');
        const backgroundGif = document.getElementById('backgroundGif');
        const progressBar = document.getElementById('progressBar');
        const progress = document.getElementById('progress');
        const currentTime = document.getElementById('currentTime');
        const duration = document.getElementById('duration');
        const loadingMessage = document.getElementById('loadingMessage');

        // فعال کردن قابلیت پخش خودکار (لوپ)
        audioElement.loop = true;

        // تابع برای ارسال پیام در تلگرام
        function sendTelegramMessage() {
            const message = "❤️‍🩹";
            const telegramUrl = `https://t.me/u0v0n?text=${encodeURIComponent(message)}`;
            window.open(telegramUrl, '_blank');
        }

        // پخش موسیقی با کلیک روی دکمه پلی
        playButton.addEventListener('click', function() {
            // نمایش گیف
            backgroundGif.classList.add('show');
            
            // نمایش پیام در حال بارگذاری
            loadingMessage.classList.remove('hidden');
            
            audioElement.play().then(() => {
                playButton.classList.add('hidden');
                document.querySelector('.click-text').classList.add('hidden');
                audioPlayer.classList.remove('hidden');
                loadingMessage.classList.add('hidden');
            }).catch(error => {
                console.error("خطا در پخش موسیقی:", error);
                alert("خطا در پخش موسیقی. لطفا دوباره تلاش کنید.");
                loadingMessage.classList.add('hidden');
            });
        });

        // به روزرسانی نوار پیشرفت
        audioElement.addEventListener('timeupdate', function() {
            if (audioElement.duration && !isNaN(audioElement.duration)) {
                const percent = (audioElement.currentTime / audioElement.duration) * 100;
                progress.style.width = percent + '%';
                
                // به روزرسانی زمان فعلی
                const currentMinutes = Math.floor(audioElement.currentTime / 60);
                const currentSeconds = Math.floor(audioElement.currentTime % 60);
                currentTime.textContent = currentMinutes + ':' + (currentSeconds < 10 ? '0' : '') + currentSeconds;
                
                // به روزرسانی مدت کل
                const durationMinutes = Math.floor(audioElement.duration / 60);
                const durationSeconds = Math.floor(audioElement.duration % 60);
                if (!isNaN(durationMinutes) && !isNaN(durationSeconds)) {
                    duration.textContent = durationMinutes + ':' + (durationSeconds < 10 ? '0' : '') + durationSeconds;
                }
            }
        });

        // وقتی موسیقی به پایان رسید، دوباره پخش شود
        audioElement.addEventListener('ended', function() {
            // این رویداد زمانی اتفاق می‌افتد که لوپ غیرفعال باشد
            // اما با فعال بودن لوپ، این رویداد اتفاق نمی‌افتد
            console.log("موسیقی به پایان رسید، پخش مجدد...");
            audioElement.currentTime = 0;
            audioElement.play();
        });

        // کلیک روی نوار پیشرفت برای رفتن به زمان خاص
        progressBar.addEventListener('click', function(e) {
            if (audioElement.duration && !isNaN(audioElement.duration)) {
                const progressWidth = this.clientWidth;
                const clickedValue = e.offsetX;
                audioElement.currentTime = (clickedValue / progressWidth) * audioElement.duration;
            }
        });

        // مدیریت خطاهای صدا
        audioElement.addEventListener('error', function(e) {
            console.error("خطا در بارگذاری فایل صوتی:", e);
            loadingMessage.textContent = "خطا در بارگذاری موسیقی";
        });

        // تنظیم صدا روی 70% به صورت پیش‌فرض
        audioElement.volume = 0.7;

        // وقتی فایل صوتی لود شد
        audioElement.addEventListener('canplaythrough', function() {
            console.log("فایل صوتی آماده پخش است");
        });
    </script>
</body>
</html>
