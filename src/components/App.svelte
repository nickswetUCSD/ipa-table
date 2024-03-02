<script>
import Visualizer from '../components/Visualizer.svelte'
import * as d3 from 'd3';
import { onMount } from 'svelte'
import { base } from '$app/paths'
export let sound = null;


// onMount(() => {
//     window.AudioContext = window.AudioContext || window.webkitAudioContext;

//     let audioContext = new AudioContext();
// })

let last_clicked_row;
let last_clicked_col;
let last_clicked_voic;

const circle_radius = 8;
const cols_with_lats = ['alv','retro','pal','vel'];

function audioPlay(fn){
    sound = document.getElementById('cons');
    sound.src = fn;
    sound.play();
}

function imgUpdate(fn){
    img_src = fn
}
function clicking(row, col, voic){
    last_clicked_row = row;
    last_clicked_col = col;
    last_clicked_voic = voic;

    let fileName_aud = "/src/components/media/consonants/" + row + '-' + col + '-' + voic + '.wav';
    let fileName_img = "/src/components/media/images/viz_" + col + '.png';

    audioPlay(fileName_aud)
    if  (['dent', 'labdent', 'phar'].includes(col)){
        fileName_img = "/src/components/media/images/viz_default.png"
    }
    imgUpdate(fileName_img)
    // console.log(row, col, voic)
}
let img_src = "/src/components/media/images/viz_default.png"

</script>

<!-- FONT DATA -->
<head>
</head>


<!-- TABLE -->
<audio id= 'cons'></audio>
<body>
    <table>
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
            <td><button on:click = {() => clicking('plos','bilab', 'v')}>b</button></td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td class = 'right'><button on:click = {() => clicking('plos','alv', 'unv')}>t</button></td>
            <td><button on:click = {() => clicking('plos','alv', 'v')}>d</button> </td>
            <td> </td>
            <td> </td>
            <td class = 'right'><button on:click = {() => clicking('plos','retro', 'unv')}>ʈ</button></td>
            <td><button on:click = {() => clicking('plos','retro', 'v')}>ɖ</button></td>
            <td><button on:click = {() => clicking('plos','pal', 'unv')}>c</button></td>
            <td><button on:click = {() => clicking('plos','pal', 'v')}>ɟ</button></td>
            <td><button on:click = {() => clicking('plos','vel', 'unv')}>k</button></td>
            <td><button on:click = {() => clicking('plos','vel', 'v')}>g</button></td>
            <td><button on:click = {() => clicking('plos','uvu', 'unv')}>q</button></td>
            <td><button on:click = {() => clicking('plos','uvu', 'v')}>ɢ</button></td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td><button on:click = {() => clicking('plos','glot', 'v')}>ʔ</button></td>
        </tr>
        <tr attr=''>
            <td class='rowlabel'> Nasal</td>
            <td> </td>
            <td><button on:click = {() => clicking('nas','bilab', 'v')}> m </button></td>
            <td> </td>
            <td> <button on:click = {() => clicking('nas','labdent', 'v')}>ɱ </button></td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td><button on:click = {() => clicking('nas','alv', 'v')}> n </button></td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td><button on:click = {() => clicking('nas','retro', 'v')}> ɳ </button></td>
            <td> </td>
            <td> <button on:click = {() => clicking('nas','pal', 'v')}>ɲ </button></td>
            <td> </td>
            <td><button on:click = {() => clicking('nas','vel', 'v')}> ŋ</button></td>
            <td> </td>
            <td><button on:click = {() => clicking('nas','uvu', 'v')}> ɴ</button></td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
        </tr>
        <tr>
            <td class='rowlabel'> Trill</td>
            <td> </td>
            <td> <button on:click = {() => clicking('trill','bilab', 'v')}>ʙ</button></td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> <button on:click = {() => clicking('trill','alv', 'v')}>r</button></td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td><button on:click = {() => clicking('trill','uvu', 'v')}> ʀ </button></td>
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
            <td><button on:click = {() => clicking('tap','alv', 'v')}> ɾ </button></td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> <button on:click = {() => clicking('tap','retro', 'v')}>ɽ </button></td>
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
            <td> <button on:click = {() => clicking('fric','bilab', 'v')}> β</button></td>
            <td class = 'right'> <button on:click = {() => clicking('fric','labdent', 'unv')}>f</button></td>
            <td> <button on:click = {() => clicking('fric','labdent', 'v')}>v</button></td>
            <td> <button on:click = {() => clicking('fric','dent', 'unv')}>θ</button></td>
            <td> <button on:click = {() => clicking('fric','dent', 'v')}>ð</button></td>
            <td class = 'right'> <button on:click = {() => clicking('fric','alv', 'unv')}>s</button></td>
            <td> <button on:click = {() => clicking('fric','alv', 'v')}>z</button></td>
            <td class ='right'> <button on:click = {() => clicking('fric','palv', 'unv')}>ʃ</button></td>
            <td> <button on:click = {() => clicking('fric','palv', 'v')}>ʒ</button></td>
            <td class = 'right'> <button on:click = {() => clicking('fric','retro', 'unv')}>ʂ</button></td>
            <td> <button on:click = {() => clicking('fric','retro', 'v')}>ʐ</button></td>
            <td> <button on:click = {() => clicking('fric','pal', 'unv')}>ç</button></td>
            <td> <button on:click = {() => clicking('fric','pal', 'v')}>ʝ</button></td>
            <td> <button on:click = {() => clicking('fric','vel', 'unv')}>x</button></td>
            <td> <button on:click = {() => clicking('fric','vel', 'v')}>ɣ</button></td>
            <td> <button on:click = {() => clicking('fric','uvu', 'unv')}>χ</button></td>
            <td> <button on:click = {() => clicking('fric','uvu', 'v')}>ʁ</button></td>
            <td class = 'right'> <button on:click = {() => clicking('fric','phar', 'unv')}>ħ</button></td>
            <td> <button on:click = {() => clicking('fric','phar', 'v')}>ʕ</button></td>
            <td><button on:click = {() => clicking('fric','glot', 'unv')}> h</button></td>
            <td><button on:click = {() => clicking('fric','glot', 'v')}> ɦ</button></td>
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
            <td> <button on:click = {() => clicking('latfric','alv', 'v')}> ɮ</button></td>
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
            <td> <button on:click = {() => clicking('approx','labdent', 'v')}>ʋ</button></td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> <button on:click = {() => clicking('approx','alv', 'v')}>ɹ</button></td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> <button on:click = {() => clicking('approx','retro', 'v')}>ɻ</button></td>
            <td> </td>
            <td> <button on:click = {() => clicking('approx','pal', 'v')}>j</button></td>
            <td> </td>
            <td> <button on:click = {() => clicking('approx','vel', 'v')}> ɰ</button></td>
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
            <td> <button on:click = {() => clicking('latapp','alv', 'v')}>l</button></td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> <button on:click = {() => clicking('latapp','retro', 'v')}> ɭ</button></td>
            <td> </td>
            <td> <button on:click = {() => clicking('latapp','pal', 'v')}>ʎ</button></td>
            <td> </td>
            <td> <button on:click = {() => clicking('latapp','vel', 'v')}>ʟ</button></td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
        </tr>
    </table>

    <!-- <Visualizer {sound}/> -->

    <!---img... dont know why its not working-->
    <!-- what you mean?? it works awesome! great work! -->
    <img src = {base + img_src} class = 'con_image' alt = "oops">

    <!-- CIRCLE DRAWING -->
    <svg>
        {#if last_clicked_col == 'bilab'}
        <g class={last_clicked_row}>
            <circle r={circle_radius} cx='36' cy='135' fill=#242331>
            </circle>
        </g>
        {/if}
        {#if last_clicked_col == 'labdent'}
        <g class={last_clicked_row}>
            <circle r={circle_radius} cx='50' cy='130'/>
        </g>
        {/if}
        {#if last_clicked_col == 'dent'}
        <g class={last_clicked_row}>
            <circle r={circle_radius} cx='50' cy='130'/>
        </g>
        {/if}
        {#if last_clicked_col == 'alv'}
        <g class={last_clicked_row}>
            <!-- Special latfric/latapp shapes -->
            {#if ((last_clicked_row != 'latfric') && (last_clicked_row != 'latapp'))}
            <circle r={circle_radius} cx='75' cy='100'/>
            {/if}
            {#if ((last_clicked_row == 'latfric') | (last_clicked_row == 'latapp'))}
                <path d="M75,90 a1,1 0 0,0 0 18,18" />
            {/if}
        </g>
        {/if}
        {#if last_clicked_col == 'palv'}
        <g class={last_clicked_row}>
            <circle r={circle_radius} cx='90' cy='90'/>
        </g>
        {/if}
        {#if last_clicked_col == 'retro'}
        <g class={last_clicked_row}>
            <!-- Special latfric/latapp shapes -->
            {#if ((last_clicked_row != 'latfric') && (last_clicked_row != 'latapp'))}
            <circle r={circle_radius} cx='100' cy='85'/>
            {/if}
            {#if ((last_clicked_row == 'latfric') | (last_clicked_row == 'latapp'))}
                <path d="M102,76 a1,1 0 0,0 0 18,18" />
            {/if}
        </g>
        {/if}
        {#if last_clicked_col == 'pal'}
        <g class={last_clicked_row}>
            <!-- Special latfric/latapp shapes -->
            {#if ((last_clicked_row != 'latfric') && (last_clicked_row != 'latapp'))}
            <circle r={circle_radius} cx='120' cy='80'/>
            {/if}
            {#if ((last_clicked_row == 'latfric') | (last_clicked_row == 'latapp'))}
                <path d="M122,69 a1,1 0 0,0 0 18,18" />
            {/if}
        </g>
        {/if}
        {#if last_clicked_col == 'vel'}
        <g class={last_clicked_row}>
            <!-- Special latfric/latapp shapes -->
            {#if ((last_clicked_row != 'latfric') && (last_clicked_row != 'latapp'))}
            <circle r={circle_radius} cx='170' cy='85'/>
            {/if}
            {#if ((last_clicked_row == 'latfric') | (last_clicked_row == 'latapp'))}
                <path d="M172,76 a1,1 0 0,0 0 18,18" />
            {/if}
        </g>
        {/if}
        {#if last_clicked_col == 'uvu'}
        <g class={last_clicked_row}>
            <circle r={circle_radius} cx='185' cy='110'/>
        </g>
        {/if}
        {#if last_clicked_col == 'phar'}
        <g class={last_clicked_row}>
            <circle r={circle_radius} cx='220' cy='170'/>
        </g>
        {/if}
        {#if last_clicked_col == 'glot'}
        <g class={last_clicked_row}>
            <circle r={circle_radius} cx='210' cy='220'/>
        </g>
        {/if}

        <!-- Extra SVG Circle for the 'nasal' animation -->
        {#if last_clicked_row == 'nas'}
        <g class='nas_extra_nose_circle'>
            <circle r={circle_radius} cx='20' cy='70'/>
        </g>
        {/if}
    </svg>


    <!-- Dataset Hyperlink -->
    <a href='https://phonetics.ucla.edu/course/chapter1/chapter1.html'>Link to audio file dataset here </a>
</body>

<style>
    @import url('https://fonts.googleapis.com/css2?family=Noto+Sans:ital,wght@0,100..900;1,100..900&display=swap');

    .right{
        text-align: right;
    }

    .con_image:hover{
        position: absolute;
        left: 900px;
        top: 380px;
        width: 300px;
    }
    .con_image{
        position: absolute;
        left: 900px;
        top: 380px;
        width: 300px;
    }
    svg {
        position: absolute;
        width: 300px;
        height: 250px;
        left: 900px;
        top: 380px; 
    }
    body {
        background-color: #242331;
    }
    button {
        padding-left: 3px;
        padding-top: 2px;
        height: auto;
        width: 35px;
        text-align: center;
        background-color: #242331;
        border-style:solid;
        border-radius: 3px;
        color: #ffffff;
        transition: 1s;
        font-family: inherit;
        font-weight: bold;
    }
    button:hover {
        padding-left: 8px;
        padding-top: 2px;
        width: 35px;
        height: auto;
        text-align: center;
        background-color: #A27035;
        border-style:solid;
        border-color: #ffffff;
        border-radius: 3px;
        transition: 0.2s;
    }
    table tr {
        font-family: inherit;
        margin-right: 40px;
    }
    table tr:hover {
        font-family: 'Noto Sans';
        font-optical-sizing: auto;
        font-weight: bold;
        font-style: normal;
        color: #DDCA7D;
        padding-left: 4px;
        padding-top: 4px;
        background-color: rgba(255,255,255,0.1);
    }
    table td {
        font-family: 'Noto Sans';
        font-optical-sizing: auto;
        font-weight: bold;
        font-style: normal;
        padding-left: 4px;
        padding-top: 4px;
    }
    table {
        position: absolute;
        font-family: 'Noto Sans';
        font-optical-sizing: auto;
        font-style: normal;
        font-variation-settings: "wdth" 100;
        color: white;
        border: 2px solid;
        border-color: black;
        border-radius: 3px;
        padding-right: 4px;
    }
    a {
        position: absolute;
        top: 400px;
    }
    svg * { 
        transform-box: fill-box;
        transform-origin: center;
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
            translate: 10px 10px;
            transform: scale(1);
        }
        5% {
            opacity: 0.5;
            translate: 10px 10px;
            transform: scale(2);
        }
        10% {
            opacity: 0;
            translate: 10px 10px;
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
            translate: -10px -10px;
            transform: scale(1);
        }
        45% {
            opacity: 0.5;
            translate: -10px -10px;
            transform: scale(2);
        }
        50% {
            opacity: 0;
            translate: -10px -10px;
            transform: scale(3);
        }
        60% {
            opacity: 0;
            translate: 10px 10px;
            transform: scale(1);
        }
        100% {
            opacity: 0;
            translate: 10px 10px;
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
            translate: 5px 5px;
        }
        25% {
            translate: -5px 5px;
        }
        50% {
            translate: 5px 5px;
            transform: scale(1.2);
        }
        75% {
            translate: -5px 5px;
        }
        100% {
            translate: 5px 5px;
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