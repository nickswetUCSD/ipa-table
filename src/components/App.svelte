<script>
    function clicking(row,col, voic){
        let fileName = "src/media/consonants/" + row + '-' + col + '-' + voic+ '.wav';
        let sound = document.getElementById('cons');
        sound.src = fileName;
        sound.play()
        console.log(row, col, voic)
    }
</script>
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
        <tr>
            <td> Plosive </td>
            <td><button on:click = {() => clicking('plos','bilab', 'unv')}>p</button></td>
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
        <tr>
            <td> Nasal</td>
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
            <td> Trill</td>
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
            <td> Tap or Flap </td>
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
            <td> Fricative </td>
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
            <td> Lateral Fricative</td>
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
            <td> Approximant </td>
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
            <td> Lateral Approximant</td>
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


    <!---img... dont know why its not working-->
    <img src = "src/media/viz_default.png">
    <a href='https://phonetics.ucla.edu/course/chapter1/chapter1.html'>Link to audio file dataset here </a>
</body>

<style>
    .right{
        text-align: right;
    }
</style>