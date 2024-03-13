<script>
import Visualizer from '../components/Visualizer.svelte'
import * as d3 from 'd3';
import { onMount } from 'svelte'
import { base } from '$app/paths'
export let sound = null;
export let button_does_what;


// onMount(() => {
//     window.AudioContext = window.AudioContext || window.webkitAudioContext;

//     let audioContext = new AudioContext();
// })

let last_clicked_row;
let last_clicked_col;
let last_clicked_voic;
button_does_what = 'whether a consonant is voiced or not.';


// let vw = 1;
// let vh = 1;
// let svg_factor = 1;

let vw;
let vh;
let svg_factor;

// $: vw = window.innerWidth;
// $: vh = window.innerHeight;
// $: svg_factor = vw / 1023;


let cons_on = true;
let graph_title = 'Consonants';


const circle_radius = 8;
const cols_with_lats = ['alv','retro','pal','vel'];


function clicking(row, col, voic){
    last_clicked_row = row;
    last_clicked_col = col;
    last_clicked_voic = voic;
    let fileName_aud;
    let fileName_img;

    $: vw = window.innerWidth;
    $: vh = window.innerHeight;
    $: svg_factor = vw / 1023 * (2/3);


    // checking to see if its a vowel or consonant
    if (['unr', 'r'].includes(voic)){
        console.log('vowel')
        fileName_aud = 'media/audio/vowels/' + row + '-' + col + '-' + voic + '.wav'
        fileName_img = 'media/images/vowels/viz-' + row + '-' + col + '.png'
    } else{
        console.log('cons')
        fileName_aud = 'media/audio/consonants/' + row + '-' + col + '-' + voic + '.wav';
        fileName_img = 'media/images/consonants/viz-' + col + '.png';
    }
    audioPlay(fileName_aud)
    if  (['dent', 'labdent', 'phar'].includes(col)){
        fileName_img = "media/images/consonants/viz-default.png"
    }
    imgUpdate(fileName_img)
    // console.log(row, col, voic)
}

// function zoomUpdate() {
//     vw = window.innerWidth;
//     vh = window.innerHeight;
//     svg_factor = vw / 1023;

//     console.log(vw);
// }


function audioPlay(fn){
    sound = document.getElementById('cons');
    sound.src = fn;
    sound.play();
}

function imgUpdate(fn){
    img_src = fn
}

let c_opcacity = 1;
let v_opacity = 0.5;

function setCons(n){
    cons_on = n;
    var r = document.querySelector(':root');
    if (n) {
        button_does_what = 'whether a consonant is voiced or not.';
        r.style.setProperty('--c_opacity', 1);
        r.style.setProperty('--v_opacity', 0.25);
    } else {
        button_does_what = 'whether a vowel is rounded or not.';
        r.style.setProperty('--c_opacity', 0.25);
        r.style.setProperty('--v_opacity', 1);
    }

}

let img_src = "media/images/consonants/viz-default.png";

</script>

<!-- TABLE -->
<audio id= 'cons'></audio>

<body>
    <button class= 'c-switch' on:click = {() =>setCons(true) } > Consonants</button>
    <button class= 'v-switch' on:click = {() =>setCons(false)} > Vowels</button>
    <!-- consonant table -->
    {#if cons_on}
    <table class="table-c">
        <tr>
            <th></th>
            <th colspan = '2'> Bilabial</th>
            <th colspan = '2'> Labio-dental</th>
            <th colspan = '2'> Dental </th>
            <th colspan = '2'> Alveolar</th>
            <th colspan = '2'> Post-Alvelar</th>
            <th colspan = '2'> Retroflex</th>
            <th colspan = '2'> Palatal</th>
            <th colspan = '2'> Velar</th>
            <th colspan = '2'> Uvular</th>
            <th colspan = '2'> Pharyngeal</th>
            <th colspan = '2'> Glottal</th>
        </tr>
        <tr attr='plos'>
            <td class='rowlabel'> Plosive </td>
            <td col = 1><button on:click = {() => clicking('plos','bilab', 'unv')}>p</button></td>
            <td><button class = 'round' on:click = {() => clicking('plos','bilab', 'v')}>b</button></td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td class = 'right'><button on:click = {() => clicking('plos','alv', 'unv')}>t</button></td>
            <td><button class = 'round' on:click = {() => clicking('plos','alv', 'v')}>d</button> </td>
            <td> </td>
            <td> </td>
            <td class = 'right'><button on:click = {() => clicking('plos','retro', 'unv')}>ʈ</button></td>
            <td><button class = 'round' on:click = {() => clicking('plos','retro', 'v')}>ɖ</button></td>
            <td><button on:click = {() => clicking('plos','pal', 'unv')}>c</button></td>
            <td><button class = 'round' on:click = {() => clicking('plos','pal', 'v')}>ɟ</button></td>
            <td><button on:click = {() => clicking('plos','vel', 'unv')}>k</button></td>
            <td><button class = 'round' on:click = {() => clicking('plos','vel', 'v')}>g</button></td>
            <td><button on:click = {() => clicking('plos','uvu', 'unv')}>q</button></td>
            <td><button class = 'round' on:click = {() => clicking('plos','uvu', 'v')}>ɢ</button></td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td><button class = 'round' on:click = {() => clicking('plos','glot', 'v')}>ʔ</button></td>
        </tr>
        <tr attr=''>
            <td class='rowlabel'> Nasal</td>
            <td> </td>
            <td><button class = 'round' on:click = {() => clicking('nas','bilab', 'v')}> m </button></td>
            <td> </td>
            <td> <button class = 'round' on:click = {() => clicking('nas','labdent', 'v')}>ɱ </button></td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td><button class = 'round' on:click = {() => clicking('nas','alv', 'v')}> n </button></td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td><button class = 'round' on:click = {() => clicking('nas','retro', 'v')}> ɳ </button></td>
            <td> </td>
            <td> <button class = 'round' on:click = {() => clicking('nas','pal', 'v')}>ɲ </button></td>
            <td> </td>
            <td><button class = 'round' on:click = {() => clicking('nas','vel', 'v')}> ŋ</button></td>
            <td> </td>
            <td><button class = 'round' on:click = {() => clicking('nas','uvu', 'v')}> ɴ</button></td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
        </tr>
        <tr>
            <td class='rowlabel'> Trill</td>
            <td> </td>
            <td> <button class = 'round' on:click = {() => clicking('trill','bilab', 'v')}>ʙ</button></td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> <button class = 'round' on:click = {() => clicking('trill','alv', 'v')}>r</button></td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td><button class = 'round' on:click = {() => clicking('trill','uvu', 'v')}> ʀ </button></td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
        </tr>
        <tr>
            <td class='rowlabel'> Tap or Flap </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td><button class = 'round' on:click = {() => clicking('tap','alv', 'v')}> ɾ </button></td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> <button class = 'round' on:click = {() => clicking('tap','retro', 'v')}>ɽ </button></td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
        </tr>
        <tr>
            <td class='rowlabel'> Fricative </td>
            <td> <button on:click = {() => clicking('fric','bilab', 'unv')}>ɸ </button></td>
            <td> <button class = 'round' on:click = {() => clicking('fric','bilab', 'v')}> β</button></td>
            <td class = 'right'> <button on:click = {() => clicking('fric','labdent', 'unv')}>f</button></td>
            <td> <button class = 'round' on:click = {() => clicking('fric','labdent', 'v')}>v</button></td>
            <td> <button on:click = {() => clicking('fric','dent', 'unv')}>θ</button></td>
            <td> <button class = 'round' on:click = {() => clicking('fric','dent', 'v')}>ð</button></td>
            <td class = 'right'> <button on:click = {() => clicking('fric','alv', 'unv')}>s</button></td>
            <td> <button class = 'round' on:click = {() => clicking('fric','alv', 'v')}>z</button></td>
            <td class ='right'> <button on:click = {() => clicking('fric','palv', 'unv')}>ʃ</button></td>
            <td> <button class = 'round' on:click = {() => clicking('fric','palv', 'v')}>ʒ</button></td>
            <td class = 'right'> <button on:click = {() => clicking('fric','retro', 'unv')}>ʂ</button></td>
            <td> <button class = 'round' on:click = {() => clicking('fric','retro', 'v')}>ʐ</button></td>
            <td> <button on:click = {() => clicking('fric','pal', 'unv')}>ç</button></td>
            <td> <button class = 'round' on:click = {() => clicking('fric','pal', 'v')}>ʝ</button></td>
            <td> <button on:click = {() => clicking('fric','vel', 'unv')}>x</button></td>
            <td> <button class = 'round' on:click = {() => clicking('fric','vel', 'v')}>ɣ</button></td>
            <td> <button on:click = {() => clicking('fric','uvu', 'unv')}>χ</button></td>
            <td> <button class = 'round' on:click = {() => clicking('fric','uvu', 'v')}>ʁ</button></td>
            <td class = 'right'> <button on:click = {() => clicking('fric','phar', 'unv')}>ħ</button></td>
            <td> <button class = 'round' on:click = {() => clicking('fric','phar', 'v')}>ʕ</button></td>
            <td><button on:click = {() => clicking('fric','glot', 'unv')}> h</button></td>
            <td><button class = 'round' on:click = {() => clicking('fric','glot', 'v')}> ɦ</button></td>
        </tr>
        <tr>
            <td class='rowlabel'> Lateral Fricative</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td class = 'right'> <button on:click = {() => clicking('latfric','alv', 'unv')}>ɬ</button></td>
            <td> <button class = 'round' on:click = {() => clicking('latfric','alv', 'v')}> ɮ</button></td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
        </tr>
        <tr>
            <td class='rowlabel'> Approximant </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> <button class = 'round' on:click = {() => clicking('approx','labdent', 'v')}>ʋ</button></td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> <button class = 'round' on:click = {() => clicking('approx','alv', 'v')}>ɹ</button></td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> <button class = 'round' on:click = {() => clicking('approx','retro', 'v')}>ɻ</button></td>
            <td> </td>
            <td> <button class = 'round' on:click = {() => clicking('approx','pal', 'v')}>j</button></td>
            <td> </td>
            <td> <button class = 'round' on:click = {() => clicking('approx','vel', 'v')}> ɰ</button></td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
        </tr>
        <tr>
            <td class='rowlabel'> Lateral Approximant</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> <button class = 'round' on:click = {() => clicking('latapp','alv', 'v')}>l</button></td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> <button class = 'round' on:click = {() => clicking('latapp','retro', 'v')}> ɭ</button></td>
            <td> </td>
            <td> <button class = 'round' on:click = {() => clicking('latapp','pal', 'v')}>ʎ</button></td>
            <td> </td>
            <td> <button class = 'round' on:click = {() => clicking('latapp','vel', 'v')}>ʟ</button></td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
        </tr>
    </table>

    <!-- vowel table -->
    {:else}
    <table class='table-v'>
        <tr>
            <th></th>
            <th colspan = '2'> Front</th>
            <th colspan = '6'> </th>
            <th colspan = '2'> Central</th>
            <th colspan = '6'> </th>
            <th colspan = '2'> Back</th>
        </tr>
        <tr attr='close'>
            <td class='rowlabel'> Close </td>
            <td col = 1><button on:click = {() => clicking('close','front', 'unr')}>i</button></td>
            <td><button class = 'round' on:click = {() => clicking('close','front', 'r')}>y</button></td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td ></td>
            <td></td>
            <td class = 'right'><button on:click = {() => clicking('close','central', 'unr')}>ɨ</button></td>
            <td><button class = 'round' on:click = {() => clicking('close','central', 'r')}>ʉ</button> </td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td> </td>
            <td> <button on:click = {() => clicking('close','back', 'unr')}>ɯ</button></td>
            <td><button class = 'round' on:click = {() => clicking('close','back', 'r')}>u</button></td>
        </tr>
        <tr attr='ccmid'>
            <td class='rowlabel'> </td>
            <td> </td>
            <td></td>
            <td> </td>
            <td></td>
            <td> <button on:click = {() => clicking('ccmid','frental', 'unr')}> ɪ </button></td>
            <td> <button class = 'round'  on:click = {() => clicking('ccmid','frental', 'r')}>ʏ</button></td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td></td>
            <td> </td>
            <td></td>
            <td> </td>
            <td></td>
            <td><button class = 'round' on:click = {() => clicking('ccmid','bentral', 'r')}> ʊ</button> </td>
            <td> </td>
            <td> </td>
            <td> </td>
        </tr>
        <tr attr = 'close-mid'>
            <td class='rowlabel'> Close-Mid </td>
            <td> </td>
            <td> </td>
            <td> <button on:click = {() => clicking('cmid','front', 'unr')}>e</button></td>
            <td> <button class = 'round'  on:click = {() => clicking('cmid','front', 'r')}>ø</button></td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> <button on:click = {() => clicking('cmid','central', 'unr')}>ǝ </button></td>
            <td> <button class = 'round'  on:click = {() => clicking('cmid','central', 'r')}>ɵ</button></td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> <button on:click = {() => clicking('cmid','back', 'unr')}> ɤ </button></td>
            <td> <button class = 'round' on:click = {() => clicking('cmid','back', 'r')}> o </button></td>
        </tr>
        <tr attr = 'mid'>
            <td class='rowlabel'>  </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td></td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> <button on:click = {() => clicking('mid','central', 'unr')}>ə </button></td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
        </tr>
        <tr>
            <td class='rowlabel'> Open-Mid </td>
            <td> </td>
            <td></td>
            <td> </td>
            <td> </td>
            <td class = 'right'> <button on:click = {() => clicking('omid','front', 'unr')}>ɛ</button></td>
            <td> <button class = 'round' on:click = {() => clicking('omid','front', 'r')}>œ</button></td>
            <td ></td>
            <td> </td>
            <td ></td>
            <td> </td>
            <td> </td>
            <td> <button on:click = {() => clicking('omid','central', 'unr')}>ɜ</button></td>
            <td> <button class = 'round' on:click = {() => clicking('omid','central', 'r')}>ɞ</button></td>
            <td></td>
            <td> </td>
            <td> </td>
            <td><button on:click = {() => clicking('omid','back', 'unr')}> ʌ</button></td>
            <td><button class = 'round' on:click = {() => clicking('omid','back', 'r')}> ɔ</button></td>
        </tr>
        <tr>
            <td class='rowlabel'> </td>
            <td></td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td></td>
            <td> </td>
            <td> <button on:click = {() => clicking('oomid','front', 'unr')}>æ</button></td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> <button on:click = {() => clicking('oomid','central', 'unr')}>ɐ</button></td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
        </tr>
        <tr>
            <td class='rowlabel'> Open </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> <button on:click = {() => clicking('open','front', 'unr')}>a</button></td>
            <td><button class = 'round' on:click = {() => clicking('open','front', 'r')}>Œ</button> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> &nbsp &nbsp &nbsp &nbsp &nbsp</td>
            <td> <button on:click = {() => clicking('open','back', 'unr')}>ɑ</button></td>
            <td> <button class = 'round' on:click = {() => clicking('open','back', 'r')}>ɒ</button></td>
        </tr>
    </table>
    {/if}

    <!-- Dataset Hyperlink -->
    <a href='https://phonetics.ucla.edu/course/chapter1/chapter1.html'>Link to audio file dataset here </a>

    <!-- SVG Drawings -->
    <svg>
        <image href = {img_src} class = 'con_image' alt = "oops"/>
        {#if last_clicked_col == 'bilab'}
        <!-- CONSONANTS -->
        <g class={last_clicked_row}>
            <circle r={circle_radius * svg_factor} cx='{36 * svg_factor}' cy='{135 * svg_factor}' fill=#242331>
            </circle>
        </g>
        {/if}
        {#if last_clicked_col == 'labdent'}
        <g class={last_clicked_row}>
            <circle r={circle_radius * svg_factor} cx='{50 * svg_factor}' cy='{130 * svg_factor}'/>
        </g>
        {/if}
        {#if last_clicked_col == 'dent'}
        <g class={last_clicked_row}>
            <circle r={circle_radius * svg_factor} cx='{50 * svg_factor}' cy='{130 * svg_factor}'/>
        </g>
        {/if}
        {#if last_clicked_col == 'alv'}
        <g class={last_clicked_row}>
            <!-- Special latfric/latapp shapes -->
            {#if ((last_clicked_row != 'latfric') && (last_clicked_row != 'latapp'))}
            <circle r={circle_radius * svg_factor} cx='{75 * svg_factor}' cy='{100 * svg_factor}'/>
            {/if}
            {#if ((last_clicked_row == 'latfric') | (last_clicked_row == 'latapp'))}
                <path d="M{75 * svg_factor}, {90 * svg_factor} a{1 * svg_factor},{1 * svg_factor} 0 0,0 0 {18 * svg_factor},{18 * svg_factor}" />
            {/if}
        </g>
        {/if}
        {#if last_clicked_col == 'palv'}
        <g class={last_clicked_row}>
            <circle r={circle_radius * svg_factor} cx='{90 * svg_factor}' cy='{90 * svg_factor}'/>
        </g>
        {/if}
        {#if last_clicked_col == 'retro'}
        <g class={last_clicked_row}>
            <!-- Special latfric/latapp shapes -->
            {#if ((last_clicked_row != 'latfric') && (last_clicked_row != 'latapp'))}
            <circle r={circle_radius * svg_factor} cx='{100 * svg_factor}' cy='{85 * svg_factor}'/>
            {/if}
            {#if ((last_clicked_row == 'latfric') | (last_clicked_row == 'latapp'))}
                <path d="M{102 * svg_factor},{76 * svg_factor} a{1 * svg_factor},{1 * svg_factor} 0 0,0 0 {18 * svg_factor},{18 * svg_factor}" />
            {/if}
        </g>
        {/if}
        {#if last_clicked_col == 'pal'}
        <g class={last_clicked_row}>
            <!-- Special latfric/latapp shapes -->
            {#if ((last_clicked_row != 'latfric') && (last_clicked_row != 'latapp'))}
            <circle r={circle_radius * svg_factor} cx='{120 * svg_factor}' cy='{80 * svg_factor}'/>
            {/if}
            {#if ((last_clicked_row == 'latfric') | (last_clicked_row == 'latapp'))}
                <path d="M{122 * svg_factor},{69 * svg_factor} a{1 * svg_factor},{1 * svg_factor} 0 0,0 0 {18 * svg_factor},{18 * svg_factor}" />
            {/if}
        </g>
        {/if}
        {#if last_clicked_col == 'vel'}
        <g class={last_clicked_row}>
            <!-- Special latfric/latapp shapes -->
            {#if ((last_clicked_row != 'latfric') && (last_clicked_row != 'latapp'))}
            <circle r={circle_radius * svg_factor} cx='{170 * svg_factor}' cy='{85 * svg_factor}'/>
            {/if}
            {#if ((last_clicked_row == 'latfric') | (last_clicked_row == 'latapp'))}
                <path d="M{172 * svg_factor},{76 * svg_factor} a{1 * svg_factor},{1 * svg_factor} 0 0,0 0 {18 * svg_factor},{18 * svg_factor}" />
            {/if}
        </g>
        {/if}
        {#if last_clicked_col == 'uvu'}
        <g class={last_clicked_row}>
            <circle r={circle_radius * svg_factor} cx='{185 * svg_factor}' cy='{110 * svg_factor}'/>
        </g>
        {/if}
        {#if last_clicked_col == 'phar'}
        <g class={last_clicked_row}>
            <circle r={circle_radius * svg_factor} cx='{220 * svg_factor}' cy='{170 * svg_factor}'/>
        </g>
        {/if}
        {#if last_clicked_col == 'glot'}
        <g class={last_clicked_row}>
            <circle r={circle_radius * svg_factor} cx='{210 * svg_factor}' cy='{220 * svg_factor}'/>
        </g>
        {/if}

        <!-- Extra SVG Circle for the 'nasal' animation -->
        {#if last_clicked_row == 'nas'}
        <g class='nas_extra_nose_circle'>
            <circle r={circle_radius * svg_factor} cx='{20 * svg_factor}' cy='{70 * svg_factor}'/>
        </g>
        {/if}






        <!-- VOWELS -->
    </svg>

    <!-- Descriptive Text Boxes -->
        <div class='rowcol_text_box'> 
            <div class='text_box_title'>
                Row Title
            </div>
            <div class='text_box_content'>
                row description here
            </div>
            <div class='text_box_title'>
                Col Title
            </div>
            <div class='text_box_content'>
                col description here
            </div>
        </div>
        <div class='specific_text_box'> 
            <div class='text_box_title'>
                Specific sound title
            </div>
            <div class='text_box_content'>
                sound description and examples here.
                text boxes should stay the same size and have its own scroll bar for text overflow. like thisssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssss
            </div>
        </div>

</body>

<style>
    @import url('https://fonts.googleapis.com/css2?family=Noto+Sans:ital,wght@0,100..900;1,100..900&display=swap');

    :root{
        --section-size: 200vh;
        --svgleft: 68vw;
        --svgtop: 1254vh;
        --h1size: 4vh;
        --textsize: 2vh;
        --v_opacity: 0.25;
        --c_opacity: 1;
    }

    .right{
        text-align: right;
    }
    .round{
        background-color: #19173b;
    }
    
    body {
        background-color: #242331;
    }
    button {
        padding-left: 0.5vw;
        padding-right: 0.5vw;
        padding-top: 0.5vh;
        padding-bottom: 0.5vh;
        margin-left: 0vw;
        margin-right: 0vw;
        margin-top: 0vh;
        margin-bottom: 0vh;
        height: 4vh;
        width: 2vw;
        text-align: center;
        background-color: #5634BD; /*#334E58; /* #2e2b51; */
        border-style:solid;
        border-color: #000000;
        border-width: 0.15vw;
        color: #ffffff;
        border-radius: 0.25vw;
        transition: 1s;
        font-family: inherit;
        font-weight: bold;
        font-size: 2vh;
    }
    button:hover {
        padding-left: 0.5vw;
        padding-right: 0.5vw;
        padding-top: 0.5vh;
        padding-bottom: 0.5vh;
        margin-left: 0vw;
        margin-right: 0vw;
        margin-top: 0vh;
        margin-bottom: 0vh;
        height: 4vh;
        width: 2vw;
        text-align: center;
        background-color: #A27035;
        border-style:solid;
        border-color: #ffffff;
        border-radius: 0.25vw;
        transition: 0.2s;
    }
    .c-switch {
        position: absolute;
        top: calc(5 * var(--section-size) + 105vh);
        left: 8vw;
        width: 8vw;
        text-align: center;
        background-color: #5634BD; /*#334E58; /* #2e2b51; */
        color: #ffffff;
        opacity: var(--c_opacity);
        transition: 0.3s;
    }
    .c-switch:hover {
        position: absolute;
        top: calc(5 * var(--section-size) + 105vh);
        left: 8vw;
        width: 8vw;
        text-align: center;
        background-color: #5634BD; /*#334E58; /* #2e2b51; */
        color: #ffffff;
        opacity:  1;
        transition: 0.1s;
    }
    .v-switch {
        position: absolute;
        top: calc(5 * var(--section-size) + 105vh);
        left: 16vw;
        width: 5vw;
        text-align: center;
        background-color: #5634BD; /*#334E58; /* #2e2b51; */
        color: #ffffff;
        opacity: var(--v_opacity);
        transition: 0.3s;
    }
    .v-switch:hover {
        position: absolute;
        top: calc(5 * var(--section-size) + 105vh);
        left: 16vw; 
        width: 5vw;
        text-align: center;
        background-color: #5634BD; /*#334E58; /* #2e2b51; */
        color: #ffffff;
        opacity:  1;
        transition: 0.1s;
    }
    .table-c {
        font-family: 'Noto Sans';
        font-optical-sizing: auto;
        font-style: normal;
        font-variation-settings: "wdth" 100;
        font-size: 2vh;
        color: white;
        border: 0.2vw solid;
        border-color: white;
        border-radius: 0.5vw;
        padding-left: 0.5vw;
        padding-right: 0.5vw;
        padding-top: 0.5vh;
        padding-bottom: 0.5vh;
        margin-bottom: 1vh;
        margin-left: auto;
        margin-right: auto;
        background-color: #2e3246;
        border-collapse: collapse;

        position:absolute;
        top: calc(5 * var(--section-size) + 110vh);
        left: 8vw;
    }
    .table-v {
        font-family: 'Noto Sans';
        font-optical-sizing: auto;
        font-style: normal;
        font-variation-settings: "wdth" 100;
        font-size: 2vh;
        color: white;
        border: 0.2vw solid;
        border-color: white;
        border-radius: 0.5vw;
        padding-left: 0.5vw;
        padding-right: 0.5vw;
        padding-top: 0.5vh;
        padding-bottom: 0.5vh;
        margin-bottom: 1vh;
        margin-left: auto;
        margin-right: auto;
        background-color: #2e3246;
        border-collapse: collapse;

        position:absolute;
        top: calc(5 * var(--section-size) + 110vh);
        left: 16vw;
    }
    table tr {
        font-family: inherit;
        font-size: 2vh;
        padding-left: 0.5vw;
        padding-right: 0.5vw;
        padding-top: 0.5vh;
        padding-bottom: 0.5vh;
        margin-left: 1vw;
        margin-right: 1vw;
        margin-top: 0.5vh;
        margin-bottom: 0.5vh;
        white-space: nowrap;
        border: 0.15vw solid;
        border-color: rgba(0,255,255,0.2);
        border-radius: 0.5vw;

        /* border-collapse: collapse; */
    }
    table tr:hover {
        font-family: 'Noto Sans';
        font-optical-sizing: auto;
        font-weight: bold;
        font-style: normal;
        color: #efc51c;;
        padding-left: 1vw;
        padding-right: 1vw;
        padding-top: 1vh;
        padding-bottom: 1vh;
        background-color: rgba(255,255,255,0.1);
    }
    table td {
        font-family: 'Noto Sans';
        font-optical-sizing: auto;
        font-weight: bold;
        font-style: normal;
        padding-left: 0.5vw;
        padding-right: 0.5vw;
        padding-top: 1vh;
        padding-bottom: 0.5vh;
        margin-left: 1vw;
        margin-right: 1vw;
        margin-top: 0vh;
        margin-bottom: 0vh;
        border: 0.1vw solid;
        border-color: rgba(0,0,0,0);
        border-collapse: collapse;
    }
    
    a {
        position: absolute;
        top: calc(5 * var(--section-size) + 165vh);
        left: 5vw;
        font-size: 2vh;
    }

    .rowcol_text_box {
        position: absolute;
        top: calc(5 * var(--section-size) + 164vh);
        left: 22vw;
        font-size: 2.5vh;

        background-color: #efc51c;
        width: 20vw;
        height: 33vh;
        overflow-wrap: break-word;
        overflow-y: scroll;
    }

    .specific_text_box {
        position: absolute;
        top: calc(5 * var(--section-size) + 164vh);
        left: 44vw;
        font-size: 2.5vh;

        background-color: #efc51c;
        width: 20vw;
        height: 33vh;
        overflow-wrap: break-word;
        overflow-y: scroll;
    }

    .text_box_title {
        font-size: 3.5vh;
        font-weight: bold;
    }

    .text_box_content {
        font-size: 2.5vh;


    }
    .con_image{
        /* position: absolute;
        left: var(--svgleft);
        top: var(--svgtop); */
        /* width: 30vw;
        height: 30vh; */
    }
    svg {
        position: absolute;
        left: var(--svgleft);
        top: calc(5 * var(--section-size) + 165vh);
        width: 20vw;
        height: 20vw;
        
    }
    svg * { 
        transform-box: fill-box;
        transform-origin: center;
        width: 20vw;
        height: auto;
        position: absolute;
        left: 68vw;
        top: calc(5 * var(--section-size) + 165vh);
    }

    .plos {
        animation: plosAnimation 1s infinite linear;
    }
    .nas {
        animation: approxAnimation 4s infinite linear;
    }
    .nas_extra_nose_circle {
        animation: nasNoseAnimation 2s infinite linear;
    }
    .tap {
        animation: tapAnimation 1s infinite linear;
    }
    .trill {
        animation: trillAnimation 1s infinite linear;
    }
    .fric {
        animation: fricAnimation 0.2s infinite linear;
    }
    .latfric {
        animation: fricAnimation 0.2s infinite linear;
    }
    .approx {
        animation: approxAnimation 1s infinite linear;
    }
    .latapp {
        animation: approxAnimation 1s infinite linear;
    }

    @keyframes plosAnimation {
        0% {
            opacity: 1;
            transform: scale(1.5);
        }
        10% {
            opacity: 1;
            transform: scale(1);
        }
        50% {
            opacity: .2;
            
        }
        75% {
            opacity: .1;
            
        }
        100% {
            opacity: 0;
            transform: scale(7);
        }
    }

    @keyframes nasNoseAnimation {
        0% {
            opacity: 0;
        }
        49% {
            opacity: 0;
        }
        50% {
            opacity: 1;
            transform: scale(1);
        }
        60% {
            opacity: .5;
            
        }
        75% {
            opacity: .2;
            
        }
        90% {
            opacity: .1;
            
        }
        100% {
            opacity: 0;
            transform: scale(7);
        }
    }

    @keyframes trillAnimation {
        0% {
            opacity: 1;
            translate: 1vw 1vh;
            transform: scale(1);
        }
        5% {
            opacity: 0.5;
            translate: 1vw 1vh;
            transform: scale(2);
        }
        10% {
            opacity: 0;
            translate: 1vw 1vh;
            transform: scale(3);
        }
        20% {
            opacity: 1;
            translate: 0px 0px;
            transform: scale(1);
        }
        25% {
            opacity: 0.5;
            translate: 0px 0px;
            transform: scale(2);
        }
        30% {
            opacity: 0;
            translate: 0px 0px;
            transform: scale(3);
        }
        40% {
            opacity: 1;
            translate: -1vw -1vh;
            transform: scale(1);
        }
        45% {
            opacity: 0.5;
            translate: -1vw -1vh;
            transform: scale(2);
        }
        50% {
            opacity: 0;
            translate: -1vw -1vh;
            transform: scale(3);
        }
        60% {
            opacity: 0;
            translate: 1vw 1vh;
            transform: scale(1);
        }
        100% {
            opacity: 0;
            translate: 1vw 1vh;
            transform: scale(1);
        }
    }

    @keyframes tapAnimation {
        0% {
            opacity: 1;
            transform: scale(1);
        }
        5% {
            opacity: 0.5;
            transform: scale(2);
        }
        10% {
            
            transform: scale(2);
        }
        20% {
            opacity: 0;
            transform: scale(2);
        }
        100% {
            opacity: 0;
            transform: scale(1);
        }
    }

    @keyframes fricAnimation {
        0% {
            opacity: 1;
            transform: scale(0.8);
            translate: 0.5vw 0.5vh;
        }
        25% {
            translate: -0.5vw 0.5vh;
        }
        50% {
            translate: 0.5vw 0.5vh;
            transform: scale(1.2);
        }
        75% {
            translate: -0.5vw 0.5vh;
        }
        100% {
            translate: 0.5 0.5vh;
        }
    }

    @keyframes approxAnimation {
        0% {
            opacity: 0.6;
        }
        25% {
            opacity: 0.25;
        }
        50% {
            opacity: 0.6;
        }
        75% {
            opacity: 0.25;
        }
        100% {
            opacity:0.6;
        }
    }


    





</style>