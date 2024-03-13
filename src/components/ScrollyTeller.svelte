<script>
    import App from '../components/App.svelte';
    import Scroller from "@sveltejs/svelte-scroller";
    // import { button_does_what } from '../components/App.svelte';
    
    
    let count, index, offset, progress;
    let check = 'check'

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
    </div>

    <div class="foreground" slot="foreground">
        

        <section> 1
            <h1> The Sounds You Make 💬</h1>
            <p> 
                Speech and language are some of the most powerful tools humans have. </p><p>Linguists have long studied the vocal patterns and expressions made by people in order to understand why and how human speech evolved as it did. Oral and written communication exist in nearly every society across the globe, allowing people to communicate through speech and to document their ideas for later through writing. </p>
        </section>
        <section> 2 
            <p>
                In the English alphabet, there are 26 letters... but how many <i>sounds</i> are there? Even accounting for letters that can represent multiple sounds, the human mouth is capable of producing far more sounds than what the vanilla English alphabet can easily represent. 
            </p>
        </section>
        <section> 3 
            <p>
                In 1888, linguists created the International Phonetic Alphabet (IPA). The different sounds that are expressible by humans can be grouped into parts (phones), based on the region(s) of the mouth used to express these sounds. The IPA uses the individual consonant phones to determine the pronunciation of words. Unlike a traditional alphabet, The IPA is applicable to almost all known languages, and can be modified or abbreviated to fit a specific language. 
            </p>
            <p> 
                The IPA is divisible into vowels, consonants, and other symbols. For the purposes of this website, let's focus on vowels and "pulmonary" (breathing-outwards) consonants, since these are sounds that can commonly found in English.
            </p>
        </section>
        <section> 4
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

        <section> 5 
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
        <section>This is the sixth section.
            <h1 class='heading-six'>
                Mouth-Mapping The IPA Table 💬
            </h1>
            <div class='explanation'>
                <p> 
                    Below is an interactive IPA table... experiment! 
                </p>
                <ul style='margin: auto'>
                    <li>
                        Click symbols to hear them, and view the mouth for a visual representation of how the sound was created. 
                    </li>
                    <li>
                        Lighter buttons are unvoiced. Darker buttons are voiced. (change this eventually)
                    </li>
                    <li>
                        Toggle between vowel and consonant IPA tables with the two purple buttons resting above the table. 
                    </li>
                    <li> 
                        Got a favorite sound? View more information about it in the descriptive text boxes below!
                    </li>
                </ul>
            </div>
            <App />
        </section>
        <section>
        <div class='translator_description'>
            <p> 
                We take for granted just how <b>complex</b> words truly are... can you recreate them in the mouthmap? 
                <br>
                Deconstruct your favorite words with this incredible IPA Translator from <a href="https://unalengua.com/ipa-translate?ttsLocale=en-US&voiceId=Salli"> Una Lengua</a>! 
            </p>
        </div>
        <iframe class='translator' style="border:none;" src="https://unalengua.com/ipa-translate?ttsLocale=en-US&voiceId=Salli"></iframe>
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
    }

    progress[value] {
    -webkit-appearance:none;
    -moz-appearance:none;        
    appearance: none;

    --color: #efc51c; /* the progress color */
    --background: #242331; /* the background color */

    border: none;
    width: 100vw;
    height: 4vh;
    margin-left: 0vw;
    margin-right: 0vw;
    padding-left: 0vw;
    padding-right: 0vw;
    margin-top: -10vw;
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
        /* background-color: rgba(0, 0, 0, 0.2); 20% opaque */
        /* color: white; */
        /* outline: magenta solid 1vh; */
        text-align: center;
        max-width: 100vw; /* adjust at will */
        color: black;
        padding: 1em;
        margin: 0 0 2em 0;
    }

    h1 { 
    color: #efc51c;
    font-size: 6vh;
    outline-color: black;
    -webkit-text-stroke-width: 0.1vh;
    -webkit-text-stroke-color: black;
    }

    .heading-six {
        position: absolute;
        left: 24vw;
        top: 1050vh;
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
        
        font-size: 3vh;
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
        
        font-size: 3vh;
    }

    .explanation {
        position: absolute;
        left: 10vw;
        top: 1065vh;
    }

    .translator {
        position: absolute;
        left: 10vw;
        top: 1240vh;
        width: 75vw;
        height: 50vh;
    }

    .translator_description {
        position: absolute;
        left: 10vw;
        top: 1220vh;
    }
</style>