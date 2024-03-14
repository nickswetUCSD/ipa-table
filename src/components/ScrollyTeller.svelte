<script>
    import App from '../components/App.svelte';
    import Scroller from "@sveltejs/svelte-scroller";
    import { fly, draw } from "svelte/transition";
    // import { button_does_what } from '../components/App.svelte';
    
    
    let count, index, offset, progress;
    let check = 'check'
    let loaded = null;

    function setOpacity(image_id, opacity) {
        if (document.getElementById(image_id)) {
            document.getElementById(image_id).style.opacity = opacity;
        }
    }

    $: if (index) {
        if (index > 0) {
            loaded = 1;
        }
    }

    $: if (loaded) {
        if (index) {
            setOpacity('whosonfirst', 0)
        } else {
            setOpacity('whosonfirst', 0.3)
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

            <iframe class='whosonfirst' id='whosonfirst' src="https://giphy.com/embed/26vUOJ7yTuQxofK9O" width='1000vw' height='100vh' frameBorder="0" allowFullScreen></iframe ><p><a href="https://giphy.com/gifs/abbott-and-costello-whos-on-first-the-naughty-nineties-26vUOJ7yTuQxofK9O"></a>
    </div>

    <div class="foreground" slot="foreground">
        

        <section class='section_one'>
            <h1 class='heading-intro'> The Sounds You Make 💬</h1>
            <p class='intro'> 
                Linguists have long studied the vocal patterns and expressions made by people in order to understand why and how human speech evolved as it did. But oftentimes, vocal languages <b>don't</b> use our mouths to the fullest extent. </p>
        </section>
        <section>
            <h1 class='heading-english'>
                English: A Quick Example 📘
            </h1>
            <p>
                Take English, for example. In the English alphabet, there are 26 letters... but how many <i>sounds</i> are there? Even accounting for letters that can represent multiple sounds, the human mouth is capable of producing far more sounds than what the vanilla English alphabet can easily represent. 
            </p>
        </section>
        <section class='section_two'>
            <p>
                In 1888, linguists created the International Phonetic Alphabet (IPA). The different sounds that are expressible by humans can be grouped into parts (phones), based on the region(s) of the mouth used to express these sounds. The IPA uses the individual consonant phones to determine the pronunciation of words. Unlike a traditional alphabet, The IPA is applicable to almost all known languages, and can be modified or abbreviated to fit a specific language. 
            </p>
            <p> 
                The IPA is divisible into vowels, consonants, and other symbols. For the purposes of this website, let's focus on vowels and "pulmonary" (breathing-outwards) consonants, since these are sounds that are commonly found in English.
            </p>
        </section>
        <section class='section_three'>
            <p>
                Vowels can be defined as sounds the voice makes with an open throat. By changing the shape of the lips, teeth, and tongue, different vowels can be made.
            </p>

            <p>
                 The IPA vowel table condenses all of these      possibilities into two axes: tongue "forwardness" and tongue height. The further left a symbol is in the table, the more the tongue travels forward from the throat to create the vowel. The further up a symbol is in the table, the higher the tongue travels (eventually reaching the roof of the mouth).
            </p>

            <p>
                 When two symbols stand right next to each other in this table, the symbol on the left is performed with unrounded lips, while the symbol on the right is performed with rounded lips.
            </p>

            <p>
                The chart itself is quite literally a spatial map of how you should position your tongue to create these vowels.
            </p>
        </section>

        <section class='section_four'>
            <p>
                Consonants, on the other hand, are created by closing the lips, tongue, or teeth off to sound in different ways, creating hard, stopped sounds. They are often characterized by the place where the sound is made, the manner of the speech, and the voicing of sound itself. 
            </p>

            <p>
                The IPA consonant table has two axes: the place of articulation and the manner of articulation. 
            </p>

            <p>
                The place of articulation refers to <b>where</b> the sound being produced, and can refer to the labial (lips), dental (teeth), and alveolar (tongue/throat) regions when speaking in English. As the regions change, the tongue and focus of sound moves further back into the throat; other languages include glottal or uvular consonants, found even further back in the throat. 
            </p>

            <p>
                The manner of articulation refers to <b>how</b> the consonant is sounded. Manner can range from single plosives to multi-hit trills, with other manners existing in the form of taps (tongue), fricatives (lip movement), or nasals (sound traveling up the nose). Approximants and laterals are more precise tongue movements, where the air travels around the tip or sides of the tongue, respectively. Some articulation manners can only be performed with certain parts of the mouth.
            </p>

            <p>
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
                (<b style='color:#efc51c'>Translate</b> your favorite words into IPA using this incredible tool from <a class='una' href="https://unalengua.com/ipa-translate?ttsLocale=en-US&voiceId=Salli"> Una Lengua!</a>)
                <br>
                 &ensp; &ensp; bug → bˈʌɡ <br>
                 &ensp; strengths → stɹˈɛŋθs <br>
                 though → ðˈo͡ʊ
            </p>
        </div>
        <iframe class='translator' style="border:none;" src="https://unalengua.com/ipa-translate?ttsLocale=en-US&voiceId=Salli"></iframe>
        <h2 class='heading-final'>
            Final Thoughts 🧠
        </h2>
        <p class='final'> 
            Language is more <b>complex</b> than we know, not just in how it is structured, but in its performance. <br> It is unlikely for an inexperienced linguist to have spoken (or even percieve the differences between) all the sounds in these tables! <br> <br>It goes to show how dedicatedly precise humans are at making mouth sounds.
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
        outline: red solid 3px;
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
         background-color: rgba(0, 0, 0, 0.2); 
        color: white;  
         outline: magenta solid 1vh;
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
        left: 36vw;
        top: 1200vh;
    }

    .heading-references {
        position: absolute;
        left: 41vw;
        top: 1400vh;
    }

    .heading-final {
        position: absolute;
        left: 41vw;
        top: 1330vh;
    }

    .heading-intro {
        position: absolute;
        left: 20vw;
        top: 30vh;
        font-size: 10vh;
    }

    .intro {
        position: absolute;
        left: 2vw;
        top: 50vh;
        font-size: 3.5vh;
        width: 90vw
    }

    .references {
        position: absolute;
        width: 90vw;
        left: 6vw;
        top: 1410vh;
        list-style-type: square;
        font-size: 2.5vh;
        text-align: left;
    }

    .final {
        position: absolute;
        width: 90vw;
        left: 2vw;
        top: 1336vh;
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
        
        font-size: 3vh;
    }

    .explanation {
        position: absolute;
        left: 15vw;
        top: 1065vh;
    }

    .translator {
        position: absolute;
        left: 10vw;
        top: 1240vh;
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
        top: 1210vh;
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

    
</style>