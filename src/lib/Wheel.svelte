<script>
    import crown from '$lib/crown.svg'
    import tada from '$lib/tada.wav'
    import firecracker from '$lib/firecracker.wav'
    export let list
    let colors = ["#ff77aa", "#ffbbee", "#ff3377", "#ff99cc", "#ff5588", "#ffbbee", "#ff3377", "#ff99cc", "#ff5588", "#ffbbee"]

    // Rotation support variables
    let rotation=0;
	let rotationDuration=0;
	let spinning = false;
    let middle = 0;
    let speed = 0

    // Refs to attatch on wheel
    let circleRef
    let refs=[]
    let tadaRef
    let fireCrackerRef

    // Wheel dimension variables
    let rootFontSize = window.getComputedStyle(document.body).getPropertyValue('font-size').toString()
    let radius=12*parseInt(rootFontSize?.slice(0, rootFontSize?.length - 2))
    $: nSlices = list?.length 
    $: angleInDegree = 360/nSlices
    $: angleInRadian = (((180-angleInDegree)/2)*Math.PI)/180
    $: sliceWidth = (2*Math.PI*radius)/nSlices
    $: sliceLength = Math.abs(Math.tan(angleInRadian)*(sliceWidth/2))
    $: percentageCut = ((radius-sliceLength)/radius)*100
    console.log(window.getComputedStyle(document.body).getPropertyValue('font-size'))

    // Winner
    let answer = ''
    let announceWinner = false



    const getDistance = (x1, y1, x2, y2) => {
        return Math.sqrt(Math.pow(x2-x1, 2)+ Math.pow(y2-y1, 2))
    }

    function getPoint(x,y,ang, dist){
        const angl= (ang*Math.PI)/180
        return {x: (x+(dist*Math.cos(angl))), y:(y+(dist*Math.sin(angl)))}
    }

    function getWinner(){
        const circleRect = circleRef?.getBoundingClientRect()
        const povX =  circleRect?.x+radius
        const povY = circleRect?.y+radius  
        const reference = getPoint(povX, povY, 0, radius)
        answer=undefined
        let minDistance = undefined
        const eachAngle = angleInDegree
        refs?.forEach((ref, idx) => {
            const newAngle = (rotation-90+(eachAngle*idx))%360
            const tempPoint = getPoint(povX, povY, newAngle, radius)
            const distance = getDistance(reference.x, reference.y, tempPoint.x, tempPoint.y)
            if(!answer || distance<minDistance)
            {
                answer = list[idx]
                minDistance = distance
            }
        })
        tadaRef.volume = 0.5
        tadaRef?.play()
        const fireCrackerTimer = setTimeout(() => {
            fireCrackerRef?.play()
            clearTimeout(fireCrackerTimer) 
        }, 1000);
    }

    function rotate(){
        if(rotationDuration <=  0 && !spinning){
            spinning=true
            answer=undefined
            speed = 1
            const duration = (Math.random()*3000)+ 5000
            rotationDuration = duration
            middle = duration/2

            const interval = setInterval(() => {
                if(spinning && speed > 0.5)
                {
                    rotation = (rotation +speed) % 360
                    speed = speed * (rotationDuration > (middle) ?1.1 :0.95)
                    rotationDuration = rotationDuration - 50
                }
                else{
                    spinning= false
                    rotationDuration=0
                    middle= 0
                    clearInterval(interval)
                    getWinner()
                }
            }, 50);
        }
    }
</script>

<audio bind:this={tadaRef} src={tada} preload="auto" />
<audio bind:this={fireCrackerRef} src={firecracker} preload="auto" />
<div class="wheel-wrapper" style="width: {2*radius}px; height: {2*radius}px; margin-top: {2}rem;" bind:this={circleRef}>
    <button class="spin-button" on:click={() => rotate()}>
        <div style="transform: rotate(-45deg); font-size:1rem;">spin</div>
    </button>
    <div class="wheel" style="transform: rotate({rotation}deg);">
        {#each list as listItem,idx}
            <div bind:this={refs[idx]} class="slice" style="background-color: {colors[idx]}; transform-origin: bottom; transform: rotate({idx*(angleInDegree)}deg); width:{sliceWidth}px; height:{radius}px; clip-path: polygon(100% 0, 100% {percentageCut}%, 50% 100%, 0 {percentageCut}%, 0 0); ">
                <div class="text-wrapper" style="transform: rotate(-90deg); font-size: {nSlices>5 ?1.25: 1.5}rem; width:{0.6*radius}px; white-space: nowrap; overflow: hidden; text-overflow: ellipsis; display: flex; justify-content:center;">{listItem?.toString()?.slice(0, 15)}</div>
            </div>
        {/each}
    </div>
</div>

<div class="answer-wrapper">
    {#if answer}
        <div class="answer">
            <img src={crown} class="crown" alt="winner"/>
            {answer}
        </div>
        <div class="stars-wrapper">
            <div class="star1 star" style="position: absolute; left: 20%; top: 10%;"/>
            <div class="star2 star" style="position: absolute; left: 50%; top: 60%;"/>
            <div class="star3 star" style="position: absolute; left: 80%; top: 10%;"/>
        </div>
    {:else}
        ???
    {/if}
</div>

<style>
    @keyframes stars-wrapper-animation{
        0% {width: 0%;}
        100% {width: 100%;}
    }
    @keyframes star-animation{
        50% {transform: translateY(1rem);}
        100% {transform: translateY(0rem);}
    }
    .wheel-wrapper{
        position: relative;
        display: flex;
        justify-content: center;
        align-items: center;
    }

    .wheel{
        width: 100%;
        height: 100%;
        position: relative;
        display: flex;
        justify-content: center;
        border-radius: 50%;
        overflow: hidden;
    }
    .slice{
        position: absolute;
        top:0;
        display: flex;
        justify-content: center;
        align-items: center;
    }

    .spin-button{
        width:50px;
        height: 50px;
        background-color: lightblue;
        border-bottom-left-radius: 50%;
        border-bottom-right-radius: 50%;
        border-top-left-radius: 50%;
        cursor: pointer;
        transform: rotate(45deg);
        position: absolute;
        z-index: 1;
        border: #966F33 0.25rem solid
    }

    .answer-wrapper{
        margin-top: 2rem;
        font-size: 2rem;
        position: relative;
    }
    .answer{
        padding: 2rem;
        background-color: lightblue;
        display: flex;
        align-items: flex-start;
        color: green;
        transform: translateX(1rem);
    }
    .crown{
        width: 3rem;
        height: 3rem;
        transform: translate(0rem, -1rem) rotate(-45deg) ;
    }
    .stars-wrapper{
        position: absolute;
        top:0;
        left:0;
        width:100%;
        height: 100%;
        animation: stars-wrapper-animation 2s ease-in-out;
    }
    .star{
        width:1rem;
        height: 1rem;
        background-color: white;
        clip-path: polygon(50% 0%, 61% 35%, 98% 35%, 68% 57%, 79% 91%, 50% 70%, 21% 91%, 32% 57%, 2% 35%, 39% 35%);
    }
    .star1{
        animation: star-animation 1s linear 2s 10 forwards;
    }
    .star2{
        animation: star-animation 1s linear 2.25s 10 forwards;
    }
    .star3{
        animation: star-animation 1s linear 2.5s 10 forwards;
    }
</style>