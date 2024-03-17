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
let lightdarktext = 'voiced';

let name_dict = {'bilab': 'Bilabial', 'labdent': 'Labio-Dental',
 'dent': 'Dental', 'alv': 'Alveolar','palv':  'Post-Alveolar', 
'retro': 'Retroflex', 'pal': 'Palatal', 'vel': 'Velar', 'uvu': 'Uvular', 
'phar': 'Pharyngeal', 'glot': 'Glottal',

'plos':'Plosive', 'nas': 'Nasal', 'trill':'Trill', 'tap': 'Tap or Flap',
'fric': 'Fricative', 'latfric': 'Lateral Fricative','approx': 'Approximant', 'latapp': 'Lateral Approximant',

'front': 'Front', 'frental': 'Near-Front', 'central': 'Central', 
'bentral': 'Near-Back', 'back': 'Back',

'close': 'Close', 'ccmid': 'Near-Close', 'cmid': 'Close-Mid', 'mid':' Mid', 
'omid': 'Open-Mid', 'oomid': 'Near-Open', 'open': 'Open'

}
let descrip_dict = {'bilab': 'Articulated with both lips. Bilabial sounds may require partial or complete stopping of air flow.'
, 'labdent': 'Sounds produced when the top teeth connect with the bottom lip. Requires stopping of air flow.'
, 'dent': 'Sounds produced when the tip of the tongue makes contact with the top teeth.'
, 'alv': 'Sounds created by pressing the tip of the tongue against the alveolar ridge, the region of the mouth where the top teeth emerge from.'
,'palv':  'Sounds expressed by the tip of the tongue touching past the alveolar ridge, close to the roof of the mouth. '
, 'retro': ' Requires curling or straightening of the tongue into flat or concave shape. Sound is produced by keeping the shaped tongue tip near the post-alveolar region. '
, 'pal': 'Sound produced by touching the middle of the tongue to the roof of the mouth (the hard palate). '
, 'vel': 'Sounds expressed by pressing the back of the tongue to the soft palate, the back of the roof of the mouth.'
, 'uvu': 'Touching the back of the tongue to the area past the roof of the mouth and into the throat. Further back than the soft palate, these sounds occur near the uvula/tonsils. '
, 'phar': 'A non-English occurring sound made by touching the root of the tongue to the back of the throat, shaping the mouth similarly to a warm breath. '
, 'glot': 'Sounds occurring fully in the throat, caused by a sudden start or stoppage of air flow.'
,

'plos':' A sudden release of air caused by built up pressure from a closed throat/mouth.'
, 'nas': ' Sounds that resonate higher up in the nasal cavity rather than the mouth. '
, 'trill':'A vibration created by holding the tongue/lips steady and keeping a consistent airflow. '
, 'tap': 'A single movement of the muscles in the mouth that produces sound.'
,'fric': ' Incomplete closure of the lips/mouth that leads to friction in the sound produced. '
, 'latfric': ' A fricative that is located unevenly between the lips and tongue. '
,'approx': 'A semi-stopped noise that is not small enough to cut out sound, resulting in a frictionless overall sound. '
, 'latapp': ' An approximant that is relegated to a specific site of the mouth. The sound is usually focused to either one or both sides of the tongue. '

,'front': 'Vowels produced by arching or curving the front of the tongue. The tongue is raised almost to the roof of the mouth.'
, 'frental': ' Sounds expressed by curving the center of the tongue higher than natural, but not quite to the roof of the mouth.'
, 'central': 'Vowels made by relaxing the lips and raising the tongue to a medium height. '
, 'bentral': 'Vowels produced by raising the back part of the tongue and keeping the lips relaxed. '
, 'back': 'Made by raising the back of the tongue and changing lip position. '
,

'close': 'The tongue is positioned near the top of the mouth, as close as possible without reducing sound. '
, 'ccmid': 'The tongue is positioned almost touching the roof of the mouth, but not as close as it can be.'
, 'cmid': 'Tongue positioned closer to the roof of the mouth, but opened a bit in comparison to closed vowels.'
, 'mid':'Sounds made when the tongue is positioned in the middle of the mouth, between relaxed and fully raised. '
, 'omid': ' Tongue positioned further open from the roof of the mouth, but not quite fully relaxed. '
, 'oomid': 'Tongue almost relaxed all the way, with a bit of tension remaining in the mouth. '
, 'open': 'Fully relaxed tongue, not touching any specific part of the mouth. '



}


let sound_desc_dict = {
    'bilab_plos_unv': '<b>p</b>ur<b>p</b>le and <b>p</b>iece.',
    'bilab_plos_v': '<b>b</b>arn, <b>b</b>ow and <b>b</b>ig.',
    'alv_plos_unv': '<b>t</b>owel, <b>t</b>ime and <b>t</b>in.',
    'alv_plos_v': '<b>d</b>o, <b>d</b>oes and <b>d</b>i<b>d</b>.',
    'vel_plos_unv': '<b>c</b>apture, <b>c</b>row, and <b>k</b>ite.',
    'vel_plos_v': '<b>g</b>rass, <b>g</b>own and <b>g</b>reen.',
    'glot_plos_unv': "the pause in uh<b>-</b>oh and borrowed names from Hawaiian such as Hawai<b>'</b>i.",

    'bilab_nas_v': '<b>m</b>o<b>m</b>, roo<b>m</b> and thu<b>mb</b>.',
    'alv_nas_v': '<b>n</b>o, <b>n</b>ever and <b>kn</b>ow.',
    'vel_nas_v': 'thi<b>ng</b>, ra<b>ng</b> and so<b>ng</b>.',
    'alv_tap_v': 'mostly when a /t/ occurs between two vowels such as bu<b>tt</b>er, la<b>t</b>er and wa<b>t</b>er.',
    'alv_approx_v': '<b>r</b>oad, <b>r</b>ive<b>r</b> and su<b>r</b>f.',
    'pal_approx_v': '<b>y</b>ellow, <b>y</b>es and <b>u</b>se.',
    'alv_latapp_v': '<b>l</b>abe<b>l</b>, <b>l</b>ove and <b>l</b>aw.',

    'labdent_fric_unv': '<b>f</b>an, <b>f</b>ish and tou<b>gh</b>.',
    'labdent_fric_v': '<b>v</b>iew, <b>v</b>iolin and <b>v</b>isit.',
    'dent_fric_unv': '<b>th</b>ing, <b>th</b>imble and wi<b>th</b>.',
    'dent_fric_v': '<b>th</b>ere, <b>th</b>en and o<b>th</b>er.',
    'alv_fric_unv': '<b>s</b>ize, <b>s</b>uper and mi<b>ss</b>.',
    'alv_fric_v': '<b>z</b>ap, <b>z</b>ig<b>z</b>ag and bu<b>zz</b>.',
    'palv_fric_unv': 'ru<b>sh</b>, <b>sh</b>ave and <b>sh</b>ear.',
    'palv_fric_v': 'vi<b>s</b>ion, delu<b>s</b>ion and ca<b>s</b>ual.',
    'glot_fric_unv': '<b>h</b>ow, <b>wh</b>o and <b>h</b>ere.',

    'front_close_unr': 'm<b>e</b>, s<b>ee</b> and s<b>ea</b>l.',
    'frental_ccmid_unr': 'f<b>i</b>t, <b>i</b>n and l<b>i</b>t.',
    'front_omid_unr': 'g<b>e</b>t, m<b>e</b>n and s<b>e</b>ll.',
    'front_oomid_unr': '<b>a</b>pp, c<b>a</b>t and s<b>a</b>t.',
    'back_omid_unr': 'b<b>u</b>t, c<b>u</b>p, and s<b>u</b>n.',
    'central_mid_unr': 'the unstressed syllables of th<b>e</b> and sof<b>a</b>. This sound, also called a <i>schwa</i> frequently appears alongside ɹ in b<b>ir</b>d and col<b>or</b>.',
    'back_open_r': 'b<b>o</b>t, fl<b>aw</b> and <b>o</b>n.',
    'back_omid_r': 'c<b>o</b>rn, f<b>o</b>re and b<b>o</b>y.',
    'bentral_ccmid_r': 'b<b>oo</b>k, h<b>oo</b>d and p<b>u</b>t.',
    'back_close_r': 'g<b>oo</b>se, b<b>oo</b>t and l<b>oo</b>.',
    'front_cmid_unr': 'pl<b>ay</b>, m<b>ay</b> and tr<b>ai</b>l, all of which this sound appears alongside /j/.',
    'back_cmid_r': 'b<b>oa</b>t, wr<b>o</b>te and p<b>o</b>pe, all of which this sound appears alongside /w/.',
    'front_open_unr': 'h<b>ow</b>, c<b>ow</b> and br<b>ow</b>n, all of which this sound appears alongside /w/.',
    'labdent_nas_v': 'sy<b>m</b>phony or e<b>m</b>phatic, always appearing before /f/ or /v/.',
    'bilab_fric_v': 'up<b>v</b>ote, always appearing after a bilabial sound.'
}

let foreign_desc_dict = {
    'front_close_r': '. This sound is popular in other Germanic languages such as German, Dutch and Swedish.',
    'frental_ccmid_r': '. This sound is popular in other Germanic languages such as German, Dutch and Swedish.',
    'central_close_r': '. This sound is mostly found in various dialects of languages found across Eastern Europe, Western Asia and Africa.',
    'central_close_unr': '. This sound is popular in Slavic languages such as Russian and Belarussian.',
    'back_close_unr': ', but it can be found in some unique regional English dialects. This sound is also found in various languages found in East-Asia.',
    'front_cmid_r': '. This sound is found in various other languages including German, French, Danish and Estonian.',
    'central_cmid_unr': '. This sound is found in various other languages including Estonian, Korean and Polish',
    'central_cmid_r': '. This sound is found in various other languages including Dutch, Uzbek and Russian.',
    'back_cmid_unr': '. This sound is found in various other languages including Thai, Mandarin and Estonian.',
    'front_omid_r': '. This sound is found in various other languages including French, Danish and Dutch.',
    'central_omid_unr': ', but can be found in some unique regional English dialects. This sound is also found in various languages such as Dutch, German and Romanian.',
    'central_omid_r': '. This sound is found in various other languages including Afrikaans, Irish and Navajo.',
    'central_oomid_unr': ' but can be found in some unique regional English dialects. This sound is also found in various languages such as Korean, Bengali, Lithuanian, Vietnamese and German.',
    'back_open_unr': ', but can be found in certain English dialects such as British pronunciations of p<b>a</b>lm or h<b>o</b>t. This sound is also found in various languages such as Dutch, Mandarin and Finnish.',
    'front_open_r': '. This sound is found in very few other dialects of languages including certain speakers of Danish, and certain Stockholm-variants of Swedish.',
    'retro_plos_unv':', but can be found in various dialects for t. This sound is also commonly found in other languages such as Nepali, Punjabi and Bengali',
    'retro_plos_v':', but can be found in various dialects for d. This sound is also commonly found in other languages such as Nepali, Punjabi and Bengali',
    'pal_plos_unv': '. This sound is found in various other languages including Czech and Hungarian.',
    'pal_plos_v': '. This sound is found in various other languages including Czech and Hungarian.',
    'uvu_plos_unv': ', but can be found in various dialects, such as Australian, in words like <b>c</b>aught. This sound is also found in various languages such as Arabic, Hungarian and Uzbek.',
    'uvu_plos_v': ', but can be found in various dialects, such as Australian, in words like <b>g</b>ot. This sound is also found in various languages such as Arabic, Mongolian and Turkmen.',
    'retro_nas_v': '. This sound is found in various other languages including Hindi, Swedish and Vietnamese.',
    'pal_nas_v': '. This sound is found in various other languages including Czech, Italian and Polish.',
    'uvu_nas_v': '. This sound is found in various other languages including Turkmen, Armenian and Georgian.',
    
    'bilab_trill_v': '. This sound is found in a handful of languages, such as Medumba and Ngwe in Cameroon and Pirahã in Brazil.',
    'alv_trill_v': '. This sound is found in various other languages, for example pe<b>rr</b>o in Spanish.',
    'retro_trill_v': '. This sound is found in a handful of dialects, such as the Belgian dialect of French or most dialects of German.',
    'retro_tap_v': '. This sound is found in various other languages including Bengali, Nepali and Punjabi.',
    'bilab_fric_unv': '. This sound is found in various other languages including Korean, Turkmen and Bengali.',
    'retro_fric_unv': '. This sound is found in various other languages including Nepali, Norwegian and Ukrainian.',
    'retro_fric_v': '. This sound is found in various other languages including Nepali, Norwegian and Ukrainian.',
    'pal_fric_unv': '. This sound is found in various other languages including Estonian, Finnish and Korean.',
    'pal_fric_v': '. This sound is found in various other languages including Estonian, Finnish and Korean.',
    'vel_fric_unv': '. This sound is found in various other languages including Greek, Russian, Vietnamese.',
    'vel_fric_v': '. This sound is found in various other languages including Greek, Russian, Vietnamese.',
    'vel_fric_unv': '. This sound is found in various other languages including Arabic, Armenian and Hebrew.',
    'vel_fric_v': '. This sound is found in various other languages including Arabic, Armenian and Hebrew.',
    'phar_fric_unv': '. This sound is found in various other languages including Arabic and Hebrew.',
    'phar_fric_v': '. This sound is found in various other languages including Arabic and Hebrew.',
    'glot_fric_v': ', but can be found in various dialects for h. This sound is also commonly found in other languages such as Danish, Finnish and Slovak.',
    
    'alv_latfric_unv': '. This sound is found in various other languages including Chickasaw and Navajo.',
    'alv_latfric_v': '. This sound is found in a handful of other languages including Arabic, Mongolian and Zulu.',
    'labdent_approx_v': ', but can be found in various dialects for v or r. This sound is also commonly found in other languages such as Finnish, Lao and Russian.',
    'retro_approx_v': ', but can be found in various dialects for r. This sound is also commonly found in other languages such as Mandarin or Faroese. ',
    'vel_approx_v': ', but can be found in various dialects for v or r. This sound is also commonly found in other languages such as Finnish, Lao and Russian.',
    'labdent_approx_v': '. This sound is found in various other languages, including Cherokee, Korean and Spanish.',
    'labdent_approx_v': ', but can be found in various dialects for v or r. This sound is also commonly found in other languages such as Finnish, Lao and Russian.',
    'retro_latapp_v': '. This sound is found in various other languages, including French, Korean and Swedish.',
    'pal_latapp_v': ', but can be found in various dialects in words like mi<b>ll</b>ion.  This sound is also commonly found in other languages such as Greek, Italian, Slovak. ',
    'vel_latapp_v': ', but can be found in various dialects such as Southern, in words like midd<b>l</b>e. This sound is also commonly found in a couple languages in Oceanic islands.'
}
let z_dict = {
    'r': 'Rounded', 'unr': 'Unrounded', 'v': 'Voiced', 'unv': 'Unvoiced'
}
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
let type = '';
let row_title = '\n\n🢁 Click a Button to Learn More!';
let col_title = '';
let row_desc = '';
let col_desc = '';
let sound_desc = ''
let sound_label = '\n\n🢁 Click a Button to Learn More!';
let z_label = ''

let placeholder = 'Insert IPA Here';
let button_highlighter_content = null;
let all_string_cons = 'pbtdʈɖcɟkɡqɢʔmɱnɳɲŋɴʙrʀɾɽɸβfvθðszʃʒʂʐçʝxɣχʁħʕhɦɬɮʋɹɻjɰlɭʎʟ';
let all_string_vowels = 'iyɨʉɯuɪʏʊeøɘɵɤoəɛœɜɞʌɔæɐaŒɑɒ';
let all_string = all_string_cons;
let all_symbols = all_string.split('')
let loaded = 0;


$: all_symbols = all_string.split('');
$: if (true|button_highlighter_content) {updateHighlights()};
$: if (cons_on) {updateHighlights()};

function updateHighlights() {
    if (button_highlighter_content == '') {
        unhighlight_buttons();
        for (let i = 0; i < all_symbols.length; i++) {
            let symbol_text = 'symbol_'.concat(all_symbols[i]);
            if (document.getElementById(symbol_text)) {
                document.getElementById(symbol_text).style.opacity = '1'
            }

        }
    } else if (button_highlighter_content != null){
        unhighlight_buttons();
        highlight_buttons();
    } else {

    }
}

function unhighlight_buttons() {
    for (let i = 0; i < all_symbols.length; i++) {
        unhighlight_button(all_symbols[i]);
    }
}

function highlight_buttons() {
    if (button_highlighter_content != '') {
        let symbols = button_highlighter_content.replace('g','ɡ').split('');
        
        console.log(symbols)
        for (let i = 0 ; i < symbols.length; i++) {
            if (all_symbols.includes(symbols[i])) {
                highlight_button(symbols[i]);
            }
        }
    }
}

function unhighlight_button(symbol){
    let symbol_text = 'symbol_'.concat(symbol);
    if (document.getElementById(symbol_text)) {
        document.getElementById(symbol_text).style.backgroundColor = '';document.getElementById(symbol_text).style.transition = '';
        document.getElementById(symbol_text).style.opacity = '0.1';
    }
}

function highlight_button(symbol){
    let symbol_text = 'symbol_'.concat(symbol)
    if (document.getElementById(symbol_text)) {
        document.getElementById(symbol_text).style.backgroundColor = '#d55959';
        document.getElementById(symbol_text).style.transition = '0s';
        document.getElementById(symbol_text).style.opacity = '1'
    }
}



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

    // updating descriptions
    z_label = z_dict[voic]
    col_title = name_dict[col]
    col_desc = descrip_dict[col]
    row_title = name_dict[row]
    row_desc = descrip_dict[row]
    sound_label = col + '_' + row + '_' + voic;
    if (Object.keys(sound_desc_dict).includes(sound_label)){
        sound_desc = 'English examples include ' + sound_desc_dict[sound_label]
    } else{
        if (foreign_desc_dict[sound_label]){
        sound_desc = 'There are no examples in English in a common dialect' + foreign_desc_dict[sound_label]}
        else {sound_desc = 'There are no examples in English in a common dialect.'}

    }
    document.getElementById('descrip').innerHTML = sound_desc



    // checking to see if its a vowel or consonant
    if (['unr', 'r'].includes(voic)){
        console.log('vowel')
        type = 'Vowel'
        fileName_aud = 'media/audio/vowels/' + row + '-' + col + '-' + voic + '.wav'
        fileName_img = 'media/images/vowels/viz-' + row + '-' + col + '.png'
    } else{
        console.log('cons')
        type = 'Consonant'
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
        all_string = all_string_cons;
        lightdarktext = 'voiced';
        r.style.setProperty('--c_opacity', 1);
        r.style.setProperty('--v_opacity', 0.25);
    } else {
        all_string = all_string_vowels;
        lightdarktext = 'rounded';
        r.style.setProperty('--c_opacity', 0.25);
        r.style.setProperty('--v_opacity', 1);
    }
}

let img_src = "media/images/consonants/viz-default.png";

</script>

<!-- TABLE -->
<audio id= 'cons'></audio>


<body>
    <!-- Button Highlighter-->
    <input bind:value = {button_highlighter_content} placeholder = {placeholder} on:click = {() => updateHighlights()} class='button_highlighter' type="text">


    <button class= 'c-switch' on:click = {() =>setCons(true)} on:click = {() => updateHighlights()} > Consonants</button>
    <button class= 'v-switch' on:click = {() =>setCons(false)} on:click = {() => updateHighlights()} > Vowels</button>
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
            <td col = 1>
                <button on:click = {() => clicking('plos','bilab', 'unv')} id='symbol_p'>
                    p
                </button>
            </td>
            <td>
                <button class = 'round' on:click = {() => clicking('plos','bilab', 'v')} id='symbol_b'>
                    b
                </button>
            </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td class = 'right'>
                <button on:click = {() => clicking('plos','alv', 'unv')} id='symbol_t'>
                    t
                </button>
            </td>
            <td>
                <button class = 'round' on:click = {() => clicking('plos','alv', 'v')} id='symbol_d'>
                    d
                </button> 
            </td>
            <td> </td>
            <td> </td>
            <td class = 'right'>
                <button on:click = {() => clicking('plos','retro', 'unv')} id='symbol_ʈ'>
                    ʈ
                </button>
            </td>
            <td>
                <button class = 'round' on:click = {() => clicking('plos','retro', 'v')} id='symbol_ɖ'>
                    ɖ
                </button>
            </td>
            <td>
                <button on:click = {() => clicking('plos','pal', 'unv')} id='symbol_c'>
                    c
                </button>
            </td>
            <td>
                <button class = 'round' on:click = {() => clicking('plos','pal', 'v')} id='symbol_ɟ'>
                    ɟ
                </button>
            </td>
            <td>
                <button on:click = {() => clicking('plos','vel', 'unv')
                } id='symbol_k'>
                    k
                </button>
            </td>
            <td>
                <button class = 'round' on:click = {() => clicking('plos','vel', 'v')} id='symbol_ɡ'>
                    ɡ
                </button>
            </td>
            <td>
                <button on:click = {() => clicking('plos','uvu', 'unv')} id='symbol_q'>
                    q
                </button>
            </td>
            <td>
                <button class = 'round' on:click = {() => clicking('plos','uvu', 'v')} id='symbol_ɢ'>
                    ɢ
                </button>
            </td>
            <td> </td>
            <td> </td>
            <td class = 'right'>
                <button on:click = {() => clicking('plos','glot', 'unv')} id='symbol_ʔ'>
                    ʔ
                </button>
            </td>
            <td> </td>
        </tr>
        <tr attr=''>
            <td class='rowlabel'> Nasal</td>
            <td> </td>
            <td>
                <button class = 'round' on:click = {() => clicking('nas','bilab', 'v')} id='symbol_m'>
                     m 
                    </button>
                </td>
            <td> </td>
            <td> 
                <button class = 'round' on:click = {() => clicking('nas','labdent', 'v')} id='symbol_ɱ'>
                    ɱ 
                </button>
            </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>
                <button class = 'round' on:click = {() => clicking('nas','alv', 'v')} id='symbol_n'> 
                    n
                 </button>
            </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>
                <button class = 'round' on:click = {() => clicking('nas','retro', 'v')} id='symbol_ɳ'> 
                    ɳ 
                </button>
            </td>
            <td> </td>
            <td> 
                <button class = 'round' on:click = {() => clicking('nas','pal', 'v')} id='symbol_ɲ'>
                    ɲ 
                </button>
            </td>
            <td> </td>
            <td>
                <button class = 'round' on:click = {() => clicking('nas','vel', 'v')} id='symbol_ŋ'> 
                    ŋ
                </button>
            </td>
            <td> </td>
            <td>
                <button class = 'round' on:click = {() => clicking('nas','uvu', 'v')} id='symbol_ɴ'> 
                ɴ
                </button>
            </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
        </tr>
        <tr>
            <td class='rowlabel'> Trill</td>
            <td> </td>
            <td> 
                <button class = 'round' on:click = {() => clicking('trill','bilab', 'v')} id='symbol_ʙ'>
                    ʙ
                </button></td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> 
                <button class = 'round' on:click = {() => clicking('trill','alv', 'v')} id='symbol_r'>
                    r
                </button></td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>
                <button class = 'round' on:click = {() => clicking('trill','uvu', 'v')} id='symbol_ʀ'>
                     ʀ 
                </button>
            </td>
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
            <td>
                <button class = 'round' on:click = {() => clicking('tap','alv', 'v')} id='symbol_ɾ'>
                     ɾ 
                    </button>
                </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> 
                <button class = 'round' on:click = {() => clicking('tap','retro', 'v')} id='symbol_ɽ'>
                    ɽ 
                </button>
            </td>
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
            <td> 
                <button on:click = {() => clicking('fric','bilab', 'unv')} id='symbol_ɸ'>
                    ɸ 
                </button>
            </td>
            <td> 
                <button class = 'round' on:click = {() => clicking('fric','bilab', 'v')} id='symbol_β'>
                     β
                    </button>
                </td>
            <td class = 'right'> 
                <button on:click = {() => clicking('fric','labdent', 'unv')} id='symbol_f'>
                    f
                </button>
            </td>
            <td> 
                <button class = 'round' on:click = {() => clicking('fric','labdent', 'v')} id='symbol_v'>
                    v
                </button>
            </td>
            <td> 
                <button on:click = {() => clicking('fric','dent', 'unv')} id='symbol_θ'>
                    θ
                </button>
            </td>
            <td> 
                <button class = 'round' on:click = {() => clicking('fric','dent', 'v')} id='symbol_ð'>
                    ð
                </button>
            </td>
            <td class = 'right'> 
                <button on:click = {() => clicking('fric','alv', 'unv')} id = 'symbol_s'>
                    s
                </button>
            </td>
            <td> 
                <button class = 'round' on:click = {() => clicking('fric','alv', 'v')} id='symbol_z'>
                    z
                </button>
            </td>
            <td class ='right'> 
                <button on:click = {() => clicking('fric','palv', 'unv')} id='symbol_ʃ'>
                    ʃ
                </button>
            </td>
            <td> 
                <button class = 'round' on:click = {() => clicking('fric','palv', 'v')} id='symbol_ʒ'>
                    ʒ
                </button>
            </td>
            <td class = 'right'> 
                <button on:click = {() => clicking('fric','retro', 'unv')} id='symbol_ʂ'>
                    ʂ
                </button>
            </td>
            <td>
                 <button class = 'round' on:click = {() => clicking('fric','retro', 'v')} id='symbol_ʐ'>
                    ʐ
                </button>
            </td>
            <td> 
                <button on:click = {() => clicking('fric','pal', 'unv')} id = 'symbol_ç'>
                    ç
                </button>
            </td>
            <td> 
                <button class = 'round' on:click = {() => clicking('fric','pal', 'v')} id='symbol_ʝ'>
                    ʝ
                </button>
            </td>
            <td> 
                <button on:click = {() => clicking('fric','vel', 'unv')} id = 'symbol_x'>
                    x
                </button>
            </td>
            <td>
                 <button class = 'round' on:click = {() => clicking('fric','vel', 'v')} id='symbol_ɣ'>
                    ɣ
                </button>
            </td>
            <td> 
                <button on:click = {() => clicking('fric','uvu', 'unv')} id='symbol_χ'>
                    χ
                </button>
            </td>
            <td> 
                <button class = 'round' on:click = {() => clicking('fric','uvu', 'v')} id='symbol_ʁ'>
                    ʁ
                </button>
            </td>
            <td class = 'right'> 
                <button on:click = {() => clicking('fric','phar', 'unv')} id='symbol_ħ'>
                    ħ
                </button>
            </td>
            <td> 
                <button class = 'round' on:click = {() => clicking('fric','phar', 'v')} id='symbol_ʕ'>
                    ʕ
                </button>
            </td>
            <td>
                <button on:click = {() => clicking('fric','glot', 'unv')} id='symbol_h'>
                     h
                    </button>
                </td>
            <td>
                <button class = 'round' on:click = {() => clicking('fric','glot', 'v')} id='symbol_ɦ'>
                     ɦ
                </button>
            </td>
        </tr>
        <tr>
            <td class='rowlabel'> Lateral Fricative</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td class = 'right'>
                 <button on:click = {() => clicking('latfric','alv', 'unv')} id='symbol_ɬ'>
                    ɬ
                </button>
            </td>
            <td> 
                <button class = 'round' on:click = {() => clicking('latfric','alv', 'v')} id='symbol_ɮ'>
                 ɮ
                </button>
            </td>
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
            <td> 
                <button class = 'round' on:click = {() => clicking('approx','labdent', 'v')} id='symbol_ʋ'>
                    ʋ
                </button>
            </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> 
                <button class = 'round' on:click = {() => clicking('approx','alv', 'v')} id='symbol_ɹ'>
                    ɹ
                </button>
            </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> 
                <button class = 'round' on:click = {() => clicking('approx','retro', 'v')} id='symbol_ɻ'>
                    ɻ
                </button>
            </td>
            <td> </td>
            <td> 
                <button class = 'round' on:click = {() => clicking('approx','pal', 'v')} id='symbol_j'>
                    j
                </button>
            </td>
            <td> </td>
            <td> 
                <button class = 'round' on:click = {() => clicking('approx','vel', 'v')} id='symbol_ɰ'> 
                    ɰ
                </button>
            </td>
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
            <td> 
                <button class = 'round' on:click = {() => clicking('latapp','alv', 'v')} id='symbol_l'>
                    l
                </button>
            </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> 
                <button class = 'round' on:click = {() => clicking('latapp','retro', 'v')} id='symbol_ɭ'>
                     ɭ
                    </button>
                </td>
            <td> </td>
            <td> 
                <button class = 'round' on:click = {() => clicking('latapp','pal', 'v')} id='symbol_ʎ'>
                    ʎ
                </button>
            </td>
            <td> </td>
            <td> 
                <button class = 'round' on:click = {() => clicking('latapp','vel', 'v')} id='symbol_ʟ'>
                    ʟ
                </button>
            </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
        </tr>
    </table>

    
    {:else}
    <!-- vowel table -->
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
            <td col = 1>
                <button on:click = {() => clicking('close','front', 'unr')} id='symbol_i'>
                    i
                </button>
            </td>
            <td>
                <button class = 'round' on:click = {() => clicking('close','front', 'r')} id='symbol_y'>
                    y
                </button>
            </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td ></td>
            <td></td>
            <td class = 'right'>
                <button on:click = {() => clicking('close','central', 'unr')} id='symbol_ɨ'>
                    ɨ
                </button>
            </td>
            <td>
                <button class = 'round' on:click = {() => clicking('close','central', 'r')} id='symbol_ʉ'>
                    ʉ
                </button>
            </td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td>
                <button on:click = {() => clicking('close','back', 'unr')} id='symbol_ɯ'>
                    ɯ
                </button>
            </td>
            <td>
                <button class = 'round' on:click = {() => clicking('close','back', 'r')} id='symbol_u'>
                    u
                </button>
            </td>
        </tr>
        <tr attr='ccmid'>
            <td class='rowlabel'> </td>
            <td> </td>
            <td></td>
            <td> </td>
            <td></td>
            <td> 
                <button on:click = {() => clicking('ccmid','frental', 'unr')} id='symbol_ɪ'>
                     ɪ 
                </button>
            </td>
            <td> 
                <button class = 'round'  on:click = {() => clicking('ccmid','frental', 'r')} id='symbol_ʏ'>
                    ʏ
                </button>
            </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td></td>
            <td> </td>
            <td></td>
            <td> </td>
            <td></td>
            <td>
                <button class = 'round' on:click = {() => clicking('ccmid','bentral', 'r')} id='symbol_ʊ'>
                    ʊ
                </button>
            </td>
            <td> </td>
            <td> </td>
            <td> </td>
        </tr>
        <tr attr = 'close-mid'>
            <td class='rowlabel'> Close-Mid </td>
            <td> </td>
            <td> </td>
            <td> 
                <button on:click = {() => clicking('cmid','front', 'unr')} id='symbol_e'>
                    e
                </button>
            </td>
            <td> 
                <button class = 'round'  on:click = {() => clicking('cmid','front', 'r')} id='symbol_ø'>
                    ø
                </button>
            </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> 
                <button on:click = {() => clicking('cmid','central', 'unr')} id='symbol_ɘ'>
                    ɘ 
                </button>
            </td>
            <td> 
                <button class = 'round'  on:click = {() => clicking('cmid','central', 'r')} id='symbol_ɵ'>
                    ɵ
                </button>
            </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> 
                <button on:click = {() => clicking('cmid','back', 'unr')} id='symbol_ɤ'>
                     ɤ 
                </button>
            </td>
            <td> 
                <button class = 'round' on:click = {() => clicking('cmid','back', 'r')} id='symbol_o'> 
                    o 
                </button>
            </td>
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
            <td> 
                <button on:click = {() => clicking('mid','central', 'unr')} id='symbol_ə'>
                    ə 
                </button>
            </td>
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
            <td class = 'right'>
                 <button on:click = {() => clicking('omid','front', 'unr')} id='symbol_ɛ'>
                    ɛ
                </button>
            </td>
            <td> 
                <button class = 'round' on:click = {() => clicking('omid','front', 'r')} id='symbol_œ'>
                    œ
                </button>
            </td>
            <td ></td>
            <td> </td>
            <td ></td>
            <td> </td>
            <td> </td>
            <td> 
                <button on:click = {() => clicking('omid','central', 'unr')} id='symbol_ɜ'>
                    ɜ
                </button>
            </td>
            <td> 
                <button class = 'round' on:click = {() => clicking('omid','central', 'r')} id='symbol_ɞ'>
                    ɞ
                </button>
            </td>
            <td></td>
            <td> </td>
            <td> </td>
            <td>
                <button on:click = {() => clicking('omid','back', 'unr')} id='symbol_ʌ'>
                     ʌ
                </button>
            </td>
            <td>
                <button class = 'round' on:click = {() => clicking('omid','back', 'r')} id='symbol_ɔ'>
                     ɔ
                </button>
            </td>
        </tr>
        <tr>
            <td class='rowlabel'> </td>
            <td></td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td></td>
            <td> </td>
            <td> 
                <button on:click = {() => clicking('oomid','front', 'unr')} id='symbol_æ'>
                    æ
                </button>
            </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> 
                <button on:click = {() => clicking('oomid','central', 'unr')} id='symbol_ɐ'>
                    ɐ
                </button>
            </td>
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
            <td> 
                <button on:click = {() => clicking('open','front', 'unr')} id='symbol_a'>
                    a
                </button>
            </td>
            <td>
                <button class = 'round' on:click = {() => clicking('open','front', 'r')} id='symbol_Œ'>
                    Œ
                </button>
             </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> &nbsp &nbsp &nbsp &nbsp &nbsp</td>
            <td> 
                <button on:click = {() => clicking('open','back', 'unr')} id='symbol_ɑ'>
                    ɑ
                </button>
            </td>
            <td> 
                <button class = 'round' on:click = {() => clicking('open','back', 'r')} id='symbol_ɒ'>
                    ɒ
                </button>
            </td>
        </tr>
    </table>
    {/if}

    <!--Changing lighter/darker-->
    <p class='lightdark'> Lighter buttons are <b style='color:#efc51c'>{'un'.concat(lightdarktext)}</b> sounds. Darker buttons are <b style='color:#efc51c'>{lightdarktext}</b> sounds. </p>


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
        {#if last_clicked_voic == 'r'}
        <g class='round_lips'>
            <circle r={circle_radius * 1.5 * svg_factor} cx='{40 * svg_factor}' cy='{135 * svg_factor}' fill=none stroke-width="0.6vh" stroke=black/>
        </g>
        {/if}
        {#if last_clicked_voic == 'unr'}
        <g class='unround_lips' stroke=black stroke-width="0.6vh">
            <path d='M{35 * svg_factor},{140 * svg_factor}H{40 * svg_factor},{60* svg_factor}'></path>
            <path d='M{35 * svg_factor},{125 * svg_factor}H{40 * svg_factor},{60* svg_factor}'></path>
        </g>
        {/if}
    </svg>

    <!-- Descriptive Text Boxes -->
        <div class='rowcol_text_box'> 
            <div class='text_box_title'>
                {row_title}
            </div>
            <div class='text_box_content'>
                {row_desc}
            </div>
            <div class='text_box_title'>
                {col_title}
            </div>
            <div class='text_box_content'>
                {col_desc}
            </div>
        </div>
        <div class='specific_text_box'> 
            <div class='text_box_title'>
                 {z_label} {col_title} {row_title} {type}
            </div>
            <div class='text_box_content' id = 'descrip'>
                
            </div>
        </div>

</body>

<style>
    @import url('https://fonts.googleapis.com/css2?family=Noto+Sans:ital,wght@0,100..900;1,100..900&display=swap');

    :root{
        --section-size: 200vh;
        --svgleft: 65vw;
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
    .button_highlighter {
        position:absolute;
        top: calc(5 * var(--section-size) + 115vh);
        left: 77.5vw;
        width: 8.5vw;
        
        font-weight: bold;
        font-size: 2vh;
        border-style: none;
        border-width: 0.5vh;
        border-radius: 0.5vh;
        padding-left: 0.5vw;
        padding-right: 0.5vw;
        padding-top: 0.5vh;
        padding-bottom: 0.5vh;
        transition: 0.3s;
    }

    .button_highlighter:hover {
        position:absolute;
        top: calc(5 * var(--section-size) + 115vh);
        left: 77.5vw;
        width: 8.5vw;
        
        font-weight: bold;
        color: #d55959;
        outline-color: #d55959;
        outline-width: 0.5vh;
        outline-style: solid;
        transition: 0.1s;
    }

    ::placeholder {

    }
    .lightdark {
        position:absolute;
        top: calc(5 * var(--section-size) + 114vh);
        left: 30vw;
        color: white;
        font-size: 2vh;
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
        top: calc(5 * var(--section-size) + 115vh);
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
        top: calc(5 * var(--section-size) + 115vh);
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
        top: calc(5 * var(--section-size) + 115vh);
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
        top: calc(5 * var(--section-size) + 115vh);
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
        top: calc(5 * var(--section-size) + 120vh);
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
        top: calc(5 * var(--section-size) + 120vh);
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
        border-color: rgba(213, 89, 89, 0.2);
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
        top: calc(5 * var(--section-size) + 175vh);
        left: 5vw;
        font-size: 2vh;
    }

    .rowcol_text_box {
        position: absolute;
        top: calc(5 * var(--section-size) + 174.5vh);
        left: 10vw;
        font-size: 2.5vh;

        background-color: #efc51c;
        width: 25vw;
        height: 33vh;
        overflow-wrap: break-word;
        overflow-y: scroll;
        white-space: pre-wrap;

    }

    /* these three are for the scrollbar */
    .rowcol_text_box::-webkit-scrollbar {
        width: 1vw;
    }
        
    .rowcol_text_box::-webkit-scrollbar-track {
        border-radius: 8px;
        background-color: #e7e7e7;
        border: 1px solid #cacaca;
        visibility: hidden;
    }
        
    .rowcol_text_box::-webkit-scrollbar-thumb {
        border-radius: 8px;
        background-color: #d55959;
    }

    .specific_text_box {
        position: absolute;
        top: calc(5 * var(--section-size) + 174.5vh);
        left: 37vw;
        font-size: 3vh;

        background-color: #efc51c;
        width: 25vw;
        height: 33vh;
        overflow-wrap: break-word;
        overflow-y: scroll;
        white-space: pre-wrap;
    }

     /* these three are for the scrollbar */
    .specific_text_box::-webkit-scrollbar {
        width: 1vw;
    }
        
    .specific_text_box::-webkit-scrollbar-track {
        border-radius: 8px;
        background-color: #e7e7e7;
        border: 1px solid #cacaca;
        visibility: hidden;
    }
        
    .specific_text_box::-webkit-scrollbar-thumb {
        border-radius: 8px;
        background-color: #d55959;
    }

    .text_box_title {
        font-size: 3.5vh;
        font-weight: bold;
        margin-bottom: -0.5vh;
    }

    .text_box_content {
        font-size: 2.5vh;


    }
    svg {
        position: absolute;
        left: var(--svgleft);
        top: calc(5 * var(--section-size) + 175vh);
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
        top: calc(5 * var(--section-size) + 175vh);
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
    .round_lips {
        animation: pulseAnimation 1s infinite ease-in-out;
    }
    .unround_lips {
        animation: pulseAnimation 1s infinite ease-in-out;
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

    @keyframes pulseAnimation {
        0% {
            transform: scale(1)
        }
        25% {
            
        }
        50% {
            transform: scale(1.2)
        }
        75% {
            
        }
        100% {
            transform: scale(1)
        }
    }


    





</style>