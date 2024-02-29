<script>
</script>

<body>
    <img src='../src/media/images/cat.jpg' alt='cat_photo' width="500" height="600">

    <script>
        function play() {
          var audio = document.getElementById("audio");
          audio.play();
        }
    </script>

    <input type="button" value="click for audio" onclick="play()">
    <audio id="audio" src="https://interactive-examples.mdn.mozilla.net/media/cc0-audio/t-rex-roar.mp3"></audio>

    <a href='https://phonetics.ucla.edu/course/chapter1/chapter1.html'>Link to audio file dataset here </a>
</body>