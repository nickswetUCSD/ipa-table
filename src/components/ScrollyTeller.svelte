<script>
    import App from '../components/App.svelte';
    import Scroller from "@sveltejs/svelte-scroller";
    import { fly, draw } from "svelte/transition";
    import { onMount } from 'svelte';
    // import { button_does_what } from '../components/App.svelte';
    
    
    let count, index, offset, progress;
    let check = 'check'
    let loaded = null;

    function setOpacity(image_id, opacity) {
        if (document.getElementById(image_id)) {
            document.getElementById(image_id).style.opacity = opacity;
        }
    }
    function setVolumes(volumes) {
        for (let i = 0; i< music_list.length; i++) {
            document.getElementById(music_list[i]).volume = volumes[i];
        }
    }

    let audioState = 0;
    let music_list = ['ipa-drums', 'ipa-rhodes', 'ipa-bass', 'ipa-vtrack', 'ipa-ctrack'];

    function changeAudio() {
        if (audioState == 0) {
            audioState = 1;

            setVolumes([0, 1, 0, 0, 0])
            for (let i = 0; i < music_list.length; i++) {
                document.getElementById(music_list[i]).play();
            }
        }
        else {
            audioState = 0;

            for (let i = 0; i < music_list.length; i++) {
                document.getElementById(music_list[i]).pause();
                document.getElementById(music_list[i]).currentTime = 0;
            }
        }
    }

    onMount (() => {
    console.log('mounted');});

    $: if (index) {
        if (index > 0) {
            loaded = 1;
        }
    }

    $: if (loaded) {
        if (index) {
            setOpacity('whosonfirst', 0)
            setOpacity('scrolltext', 0)
        } else {
            setOpacity('whosonfirst', 0.3)
            setOpacity('scrolltext', 1)
            setVolumes([0, 1, 0, 0, 0])
        }
        if (index == 1) {
            setOpacity('alphabet', 0.2)
            setVolumes([0, 1, 1, 0, 0])
        } else {
            setOpacity('alphabet', 0)
        }
        if (index == 2) {
            setVolumes([0.3, 1, 1, 0, 0])
        } else {
        }
        if ((index == 2)|(index == 3)|(index == 4)) {
            setOpacity('ipaBig', 0.2)
        } else {
            setOpacity('ipaBig', 0)
        }
        if (index == 3) {
            setOpacity('ipaVowels', 1)
            setVolumes([0.3, 1, 1, 1, 0])
        } else {
            setOpacity('ipaVowels', 0)
        }
        if (index == 4) {
            setVolumes([0.3, 1, 1, 0, 1])
        } else {
        }
        if ((index == 4) & (offset < 0.84)) {
            setOpacity('ipaCons', 1)
        } else {
            setOpacity('ipaCons', 0)
        }
        if (index == 5) {
            setVolumes([0.1, 0.3, 0.3, 0, 0])
        } else {
        }
        if ((index == 6)) {
            setOpacity('mouths', 0.2)
            setVolumes([0.1, 0.3, 0.3, 0, 0])
        } else {
            setOpacity('mouths', 0)
        }
        if ((index == 7)) {
            setOpacity('study', 0.2)
            setVolumes([0, 1, 0, 0, 0])
        } else {
            setOpacity('study', 0)
        }
    }




    let letters = 'abcdefghijklmnopqrstuvwxyz'.split('')
   

</script>

<body>
    <Scroller
    top={0.0}
    bottom={1}
    threshold={0.5}
    bind:count
    bind:index
    bind:offset
    bind:progress
    >
    <div class="background" slot="background">
        <div class="progress-bars">
            <div class="big-bar">
            <progress value={progress || 0} />
            </div>

            
    
            <!-- <p>current section: <strong>{index + 1}/{count}</strong></p>
            <progress value={count ? (index + 1) / count : 0} />
      
            <p>offset in current section</p>
            <progress value={offset || 0} />
      
            <p>total progress</p>
            <progress value={progress || 0} /> -->
        </div>
            <!-- Who's On First GIF -->    
            <iframe class='whosonfirst' id='whosonfirst' src="https://giphy.com/embed/26vUOJ7yTuQxofK9O"  frameBorder="0" allowFullScreen></iframe >

            <!-- Scroll Reminder -->
            <svg class='scrolltext' id='scrolltext'>
                <text class='arrow' fill='white' x="38.5vw" y="93vh" stroke='0px' style='font: 3vw sans-serif;'>
                    ↓
                </text>
                <text fill='white' x="41vw" y="93vh" stroke='0px' style='font: 2vw sans-serif;'>
                    scroll down to continue
                </text>
                <text class='arrow' fill='white' x="62vw" y="93vh"  style='font: italic 3vw sans-serif;'>
                    ↓
                </text>
            </svg>

            <!-- Alphabet SVG -->
            <svg class='alphabet' id='alphabet'>
                {#each letters as letter, i}
                <text class='letter' fill='white' x='{10 + (80 * (i % 13) / 13)}vw' y='{40 + (20 * (Math.floor(i / 13)))}vh' style='font: italic 5vw sans-serif; animation-delay:{i * 2/26}s'> {letter} </text>
                {/each}
                <text fill='white' x="2vw" y="19vh" transform=" scale(2,3)" style='font: italic 5vw sans-serif;'>
                    &lbrace;
                </text>
                <text fill='white' x="46vw" y="19vh" transform=" scale(2,3)" style='font: italic 5vw sans-serif;'>
                    &rbrace;
                </text>
                <text outline='0px'fill='white' x='20vw' y='80vh' style='font: italic 3vw sans-serif;'> These do not represent all possible sounds.</text>
            </svg>

            <!--IPA Images-->
            <img class='ipaBig' id='ipaBig' src="media/images/ipaConsonants.png"/>

            <img class='ipaVowels' id='ipaVowels' src="media/images/ipaVowels.png"/>

            <img class='ipaCons' id='ipaCons' src="media/images/ipaConsonants.png"/>

            <img class='mouths' id='mouths' src="media/images/mouths.png"/>

            <img class='study' id='study' src="media/images/study.gif"/>

    </div>

    <div class="foreground" slot="foreground">

        <audio class='music' id='pluh' loop=true src='media/audio/music/pluh.mp3'/>
        <audio class='music' id='ipa-drums' loop=true src='media/audio/music/ipa-drums.wav'/>
        <audio class='music' id='ipa-rhodes' loop=true src='media/audio/music/ipa-rhodes.wav'/>
        <audio class='music' id='ipa-bass' loop=true src='media/audio/music/ipa-bass.wav'/>
        <audio class='music' id='ipa-vtrack' loop=true src='media/audio/music/ipa-vtrack.wav'/>
        <audio class='music' id='ipa-ctrack' loop=true src='media/audio/music/ipa-ctrack.wav'/>

        <label class="switch">
            <input type="checkbox" on:change={() => changeAudio()}>
            <span class="slider round"></span>
          </label>
        

        <section class='section_one'>
            <h1 class='heading-intro'> The Sounds You Make 💬</h1>
            <p class='intro'> 
                Linguists have long studied the vocal patterns and expressions made by people in order to understand why and how human speech evolved as it did. But oftentimes, vocal languages <b>don't</b> use our mouths to the fullest extent. <br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br> (🖱️ An interactive web article by 
                <b style='color:#efc51c; -webkit-text-stroke-width: 0.1vh;-webkit-text-stroke-color: black;'>Nick Swetlin
                </b>,
                <b style='color:#efc51c; -webkit-text-stroke-width: 0.1vh;-webkit-text-stroke-color: black;'>Dante Testini
                </b>,
                and 
                <b style='color:#efc51c; -webkit-text-stroke-width: 0.1vh;-webkit-text-stroke-color: black;'>Vivek Srinivasan
                </b>) 
                <br><br> <b>Music 🎵 &ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;</b>
            </p>
        </section>
        <label class="switch">
            <input type="checkbox" on:change={() => changeAudio()}>
            <span class="slider round"></span>
          </label>
        <section>
            <h1 class='heading-english'>
                English: A Quick Example 📘
            </h1>
            <p class='desc-english'>
                Take English, for example. <br><br> In the English alphabet, there are 26 letters... but how many <i>sounds</i> are there? Even accounting for letters that can represent multiple sounds, the human mouth is capable of producing far more sounds than what the vanilla English alphabet can easily represent.  <br><br><br><br><br><br><br><br><br><br><br><br> So what gives? <br>. . .<br>Is there a way we can represent <i> all </i> sounds? Perhaps there is.
            </p>
        </section>
        <section class='section_two'>
            <p class='ipa_history_text'>
                In 1888, linguists created the <b> International Phonetic Alphabet (IPA).</b> <br><br><br><br>
                
                The different sounds that are expressible by humans can be grouped into parts (phones), based on the region(s) of the mouth used to express these sounds. The IPA uses these individual consonant phones to determine the pronunciation of words. Unlike a traditional alphabet, The IPA is applicable to <b>almost all known languages</b>, and can be modified or abbreviated to fit a specific language. <br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br>
                The IPA is divisible into vowels, consonants, and other symbols. For the purposes of this website, let's focus on <b style='color:#d55959;'>vowels</b> and "pulmonic" (breathing-outwards) <b style='color:#d55959;'>consonants</b>, since these are sounds that are commonly found in English.
            </p>
        </section>
        <section class='section_three'>
            <h1 class='heading-vowels'> IPA Vowels 🅰️ </h1>
            <p class='desc-vowels'>
                <br><br>
                <b style='color:#d55959;'>Vowels</b> can be defined as sounds the voice makes with an open throat. By changing the shape of the lips, teeth, and tongue, different vowels can be made.<br><br><br>
                 The IPA vowel table condenses all of these      possibilities into two axes:
                    <b style='color:#efc51c'>tongue "forwardness"</b> and  
                    <b style='color:#efc51c'>tongue height</b>.
                 The further left a symbol is in the table, the more the tongue travels forward from the throat to create the vowel. The further up a symbol is in the table, the higher the tongue travels (eventually reaching the roof of the mouth).<br><br><br>
                 When two symbols stand right next to each other in this table, the symbol on the left is performed with unrounded lips, while the symbol on the right is performed with rounded lips.<br><br><br><br>
                The chart itself is quite literally a spatial map of how you should position your tongue 👅 to create these vowels.
            </p>
        </section>

        <section class='section_four'>
            <h1 class='heading-cons'> IPA Consonants 🅱️ </h1>
            <p class='desc-cons'>
                <b style='color:#d55959;'>Consonants</b>, on the other hand, are created by closing the lips, tongue, or teeth off to sound in different ways, creating hard, stopped sounds. They are often characterized by the place where the sound is made, the manner of the speech, and the voicing of sound itself. <br><br><br>
                The IPA consonant table has two axes: the <b style='color:#efc51c'>place of articulation</b> and the <b style='color:#efc51c'>manner of articulation</b>. <br><br><br>
                The place of articulation refers to <b>where</b> the sound being produced, and can refer to the labial (lips), dental (teeth), and alveolar (tongue/throat) regions when speaking in English. As the regions change, the tongue and focus of sound moves further back into the throat; other languages include glottal or uvular consonants, found even further back in the throat. <br><br><br>
                The manner of articulation refers to <b>how</b> the consonant is sounded. Manner can range from single plosives to multi-hit trills, with other manners existing in the form of taps (tongue), fricatives (friction), or nasals (sound traveling up the nose). Approximants and laterals are more precise tongue movements, where the air travels around the tip or sides of the tongue, respectively. Some articulation manners can only be performed with certain parts of the mouth. <br><br><br>
                When two symbols stand right next to each other in this table, the symbol on the left is performed without voice, while the symbol on the right is performed with voice.
            </p>

        </section>
        <section>
            <h1 class='heading-mouth-map'>
                Mouth-Mapping The IPA Table 💬
            </h1>
            <div class='explanation'>
                <p> 
                    Below is an interactive IPA table... experiment! 
                </p>
                <ul style='text-align:left'>
                    <li>
                        <b style='color:#efc51c'>Click</b> buttons to hear mouth noises, and then... 
                    </li>
                    <li>
                        <b style='color:#efc51c'>Watch </b> the mouth diagram for a loose animation of how the sound was created!
                    </li>
                    <li>
                        <b style='color:#efc51c'>Toggle</b> between vowel and consonant IPA tables with buttons at the top-left of the table. 
                    </li>
                    <li> 
                        <b style='color:#efc51c'>Scroll</b>
                        through the yellow description boxes to learn more about your favorite sounds!
                    </li>
                    <li>
                        <b style='color:#efc51c'>Translate</b> words into IPA using the <a href="https://unalengua.com/ipa-translate?ttsLocale=en-US&voiceId=Salli" class='una'> Una Lengua</a> black-box at the bottom, and then...
                    </li>
                    <li>
                        <b style='color:#efc51c'>Type</b> IPA characters inside the interactive filter at the top-right of the table.
                    </li>
                </ul>
            </div>
            <App />
        </section>
        <section>
        <h2 class='heading-translator'> 
            Here's The Translator 👇 
        </h2>
        <div class='translator_description'>
            <p> 
                (<b style='color:#efc51c'>Translate</b> your favorite words into IPA using this incredible tool from <a class='una' href="https://unalengua.com/ipa-translate?ttsLocale=en-US&voiceId=Salli"> Una Lengua!</a><br> Then, go back up and plug your IPA into the table!)<br>
                <br>
                 &ensp; &ensp; bug → bˈʌɡ <br>
                 &ensp; strengths → stɹˈɛŋθs <br>
                 though → ðˈo͡ʊ
            </p>
        </div>
        <iframe class='translator' style="border:none;" src="https://unalengua.com/ipa-translate?ttsLocale=en-US&voiceId=Salli"></iframe>
        <h1 class='heading-considerations'> 
            Considerations 🧠 
        </h1>
        <div class='considerations'>
            <p> 
                The tables we have presented are just <i> one part </i> of the full IPA table. Yes, language is <i>that vast.</i> <br> We hope the phoneme animations serve as a decent approximation for what actually goes on in your mouth.
            </p>
        </div>
        </section>
        <section>
        <h1 class='heading-final'>
            Final Thoughts 💡
        </h1>
        <p class='final'> 
            Language is more <b>complex</b> than we know, not just in how it is structured, but in its performance. <br> It is difficult for the average person to pronounce (or even notice the differences between) all the sounds in these tables! <br> <br>Knowing IPA is very useful for learning new languages, and for theorizing what are possible sounds versus impossible sounds for humans to make. We hope that the next time you hear a word you don't know, you're reminded of the usefulness of the paradigm that the IPA provides!
        </p>
        <h2 class='heading-references'>
            References🔗
        </h2>
        <div>
            <ul class='references'>
                <li>
                "IPA Vowels". InternationalPhoneticAssociation.org. 2018, <a class='link' href='https://www.internationalphoneticassociation.org/content/ipa-vowels'>https://www.internationalphoneticassociation.org/content/ipa-vowels.</a>
                </li>
                <li>
                Ladefoged, Peter. A Course in Phonetics. UCLA, 2007, <a class='link' href='https://phonetics.ucla.edu/course/chapter1/chapter1.html'>https://phonetics.ucla.edu/course/chapter1/chapter1.html.
                </a>
                </li>
                <li>
                Ladefoged, Peter (2001). Vowels and consonants : an introduction to the sounds of languages. Malden, Mass.: Blackwell. ISBN 0-631-21411-9. OCLC 43434745. 
                </li>
                <li>
                Phonetic alphabet - examples of sounds. The London School of English, 2017, <a class='link' href='https://www.londonschool.com/blog/phonetic-alphabet/'>https://www.londonschool.com/blog/phonetic-alphabet/. </a>
                </li>
                <li>
                Styler, Will. Phonetics: The Sounds of Language. LIGN 101, 2024, <a class='link' href='https://wstyler.ucsd.edu/talks/l101_4_phonetics1.html#/1'>https://wstyler.ucsd.edu/talks/l101_4_phonetics1.html#/1.</a>
                </li>
                <li>
                Styler, Will. Phonetics II. LIGN 101, 2024, <a class ='link' href='https://wstyler.ucsd.edu/talks/l101_5_phonetics2.html#/'>https://wstyler.ucsd.edu/talks/l101_5_phonetics2.html#/. </a>
                </li>
                <li>
                Una Lengua IPA Translator, <a class='link'href='https://unalengua.com/ipa-translate?ttsLocale=en-US&voiceId=Salli'>https://unalengua.com/ipa-translate?ttsLocale=en-US&voiceId=Salli.</a>
                </li> 
            </ul>
        </div>

        </section>
        
    </div>
    </Scroller>
</body>

<style>

    :root {
        --slowleftscroll: 0vw;
    }

    .music {
        transition: 1s;
    }

    /* The switch - the box around the slider */
    .switch {
    position: absolute;
    display: inline-block;
    width: 4vw;
    height: 4vh;
    left: 50vw;
    top: 145vh;
    }

    /* Hide default HTML checkbox */
    .switch input {
    opacity: 0;
    width: 0;
    height: 0;
    }

    /* The slider */
    .slider {
    position: absolute;
    cursor: pointer;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background-color: #242331;
    -webkit-transition: .4s;
    transition: .4s;
    }

    .slider:before {
    position: absolute;
    content: "";
    height: 4vh;
    width: 2vw;
    background-color: white;
    -webkit-transition: .4s;
    transition: .4s;
    }

    input:checked + .slider {
    background-color: #efc51c;
    }

    /* input:focus + .slider {
    box-shadow: 0 0 1px #efc51c;
    } */

    input:checked + .slider:before {
    -webkit-transform: translateX(2vw);
    -ms-transform: translateX(2vw);
    transform: translateX(2vw);
    }

    .slider.round {
    border-radius: 2vh;
    }

    .slider.round:before {
    border-radius: 50%;
    }

    body {
        overflow-x: hidden;
        overflow-y: hidden;
        margin: 0px;
    }

    body::-webkit-scrollbar {
    display: none;
    }

    body {
    -ms-overflow-style: none;  /* IE and Edge */
    scrollbar-width: none;  /* Firefox */
    }

    .foreground {
        width: 100%;
        margin: 0 auto;
        height: auto;
        position: relative;
        /* outline: red solid 3px; */
    }

    .progress-bars {
        /* background: rgba(170, 51, 120, 0.2) /*  40% opaque */
        visibility: visible;
        transform: rotate(0.25turn);
        transform-origin: 0;
        position: absolute;
        top: -2vw;
        left: 99vw;
    }

    progress[value] {
    -webkit-appearance:none;
    -moz-appearance:none;        
    appearance: none;

    --color: #efc51c; /* the progress color */
    --background: #242331; /* the background color */

    border: none;
    width: 100vh;
    height: 4vh;
    margin-left: 0vw;
    margin-right: 0vw;
    padding-left: 0vw;
    padding-right: 0vw;
    margin-top: 0vw;
    border-radius: 0vh;
    background: var(--background);
    }

    progress[value]::-webkit-progress-bar {
    border-radius: 1vh;
    background: var(--background);
    }
    progress[value]::-webkit-progress-value {
    border-radius: 1vh;
    background: var(--color);
    }

    
    section {
        height: 200vh; /* Change section-size CSS variable in App.svelte too if you change this*/
         /* background-color: rgba(0, 0, 0, 0.2); 
        color: white;  
         outline: magenta solid 1vh; */
        text-align: center; 
        max-width: 100vw; 
        color: black;
        padding: 1vh;
        margin: 0 0 5vh 0;
    }

    h1 { 
    color: #efc51c;
    font-size: 6vh;
    outline-color: black;
    -webkit-text-stroke-width: 0.1vh;
    -webkit-text-stroke-color: black;
    }

    h2 { 
    color: #efc51c;
    font-size: 4vh;
    outline-color: black;
    -webkit-text-stroke-width: 0.1vh;
    -webkit-text-stroke-color: black;
    }

    .heading-mouth-map {
        position: absolute;
        left: 24vw;
        top: 1050vh;
    }

    .heading-translator {
        position: absolute;
        left: 37vw;
        top: 1250vh;
    }

    .heading-considerations {
        position: absolute;
        left: 34vw;
        top: 1390vh;
    }

    .heading-references {
        position: absolute;
        left: 41vw;
        top: 1600vh;
    }

    .heading-final {
        position: absolute;
        left: 35vw;
        top: 1490vh;
    }

    .heading-intro {
        position: absolute;
        left: 20vw;
        top: 20vh;
        font-size: 10vh;
    }

    .heading-english {
        position: absolute;
        left: 30vw;
        top: 230vh;
    }

    .heading-vowels {
        position: absolute;
        left: 60vw;
        top: 620vh;
    }

    .heading-cons {
        position: absolute;
        left: 57vw;
        top: 820vh;
    }

    .intro {
        position: absolute;
        left: 2vw;
        top: 40vh;
        font-size: 3.5vh;
        width: 90vw;
    }

    .desc-english {
        position: absolute;
        left: 0vw;
        top: 250vh;
    }

    .ipa_history_text {
        position: absolute;
        left: 0vw;
        top: 430vh;
    }

    .considerations {
        position: absolute;
        left: 2vw;
        top: 1405vh;
    }

    .desc-vowels {
        position: absolute;
        left: 40vw;
        top: 630vh;
    }

    .desc-cons {
        position: absolute;
        left: 40vw;
        top: 840vh;
    }

    .references {
        position: absolute;
        width: 90vw;
        left: 6vw;
        top: 1610vh;
        list-style-type: square;
        font-size: 2.5vh;
        text-align: left;
    }

    .final {
        position: absolute;
        width: 90vw;
        left: 0vw;
        top: 1504vh;
        font-size: 3.5vh;
    }

    p {
        color: #ffffff;/*#242331;*/
        /* background-color: rgba(236, 189, 72, 0.3);  */
        padding-left: 2vw;
        padding-right: 2vw;
        padding-top: 2vh;
        padding-bottom: 2vh;
        margin-left: 1vw;
        margin-right: 1vw;
        margin-top: 1vh;
        margin-bottom: 1vh;
        
        font-size: 3.5vh;
    }
    ul {
        color: #ffffff;/*#242331;*/
        /* background-color: rgba(236, 189, 72, 0.3);  */
        padding-left: 2vw;
        padding-right: 2vw;
        padding-top: 2vh;
        padding-bottom: 2vh;
        margin-left: 1vw;
        margin-right: 1vw;
        margin-top: 1vh;
        margin-bottom: 1vh;
        list-style-type: square;
        
        font-size: 3.5vh;
    }

    .explanation {
        position: absolute;
        left: 10vw;
        top: 1065vh;
    }

    .translator {
        position: absolute;
        left: 10vw;
        top: 1300vh;
        width: 75vw;
        height: 55vh;
    }

    iframe::-webkit-scrollbar {
        width: 10px;
    }
        
    iframe::-webkit-scrollbar-track {
        border-radius: 8px;
        background-color: #e7e7e7;
        border: 1px solid #cacaca;
        visibility: hidden;
    }
        
    iframe::-webkit-scrollbar-thumb {
        border-radius: 8px;
        background-color: #d55959;
    }

    .translator_description {
        position: absolute;
        left: 10vw;
        top: 1260vh;
    }

    .una {
        color: #ffffff;
        transition: 0.5s;
    }
    .una:hover {
        color:#d55959;
        transition: 0.1s;
    }

    .link {
        color:#ffffff;
        transition: 0.5s
    }
    .link:hover {
        color:#d55959;
        transition: 0.1s
    }

    .whosonfirst {
        width: 100vw;
        height: 100vh;
        opacity: 0.3;
        transition: 1s
    }

    .scrolltext {
        position: absolute;
        width: 100vw;
        height: 100vh;
        left: 0vw;
        top: 0vh;
        opacity: 1;
        transition: 1s
    }

    .arrow {
        animation: arrowAnimation 2s infinite ease-in-out;
    }

    .alphabet {
        position: absolute;
        width: 100vw;
        height: 100vh;
        left: 0vw;
        top: 0vh;
        opacity: 0;
        transition: 1s;
    }

    .mouths {
        position: absolute;
        width: 100vw;
        height: 100vh;
        left: 0vw;
        top: 0vh;
        opacity: 0;
        transition: 1s;
    }

    .ipaBig {
        position: absolute;
        width: 150vw;
        height: 150vh;
        left: var(--slowleftscroll);
        top: 0vh;
        opacity: 0;
        transition: 1s;
        animation: scrollAnimation 100s infinite linear;
    }

    .ipaVowels {
        position: absolute;
        width: 35vw;
        height: 55vh;
        left: 5vw;
        top: 20vh;
        opacity: 0;
        transition: 1s;
        object-fit: scale-down;
    }

    .ipaCons {
        position: absolute;
        width: 35vw;
        height: 55vh;
        left: 5vw;
        top: 20vh;
        opacity: 0;
        transition: 1s;
        object-fit: scale-down;
    }

    .study {
        position: absolute;
        width: 100vw;
        height: 100vh;
        left: 0vw;
        top: 0vh;
        opacity: 0;
        transition: 1s;
    }

    .letter {
        animation: letterAnimation 2s infinite ease-in-out;
    }

    @keyframes scrollAnimation {
        0% {
            left: 0vw;
            top: 0vh;
        }
        50% {
            left: -50vw;
            top: -20vh;
        }
        100% {
            left: 0vw;
            top: 0vh;
        }
    }

    @keyframes letterAnimation {
        0% {
            transform: translate(0vw);
        }
        50% {
            transform: translate(3vw);
        }
        100% {
            transform: translate(0vw);
        }
    }

    @keyframes arrowAnimation {
        0% {
            translate: 0vw 0vh;
        }
        33% {
            translate: 0vw 0vh;
        }
        50% {
            translate: 0vw 1.5vh;
        }
        66% {
            translate: 0vw 0vh;
        }
        83% {
            translate: 0vw 1.5vh;
        }
        100% {
            translate: 0vw 0vh;
        }
    }


    
</style>