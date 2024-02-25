<script>
    export let list
    let r= 240
    let colors = ["#ff77aa", "#ffbbee", "#ff3377", "#ff99cc", "#ff5588"]
    let rotation=0;
	let rotationDuration=0;
	let contentRadius = r*4/5;
	let spinning = false;
	let outerWidth=24;
    let lowest = 0;
    let highest = 0;
	$: cntSlices = list?.length
	$: sliceAngle=3.142*2/cntSlices;
	$: sliceDegrees=360/cntSlices|0;
	$: result=(((270-rotation)/sliceDegrees)+cntSlices|0)%cntSlices;


    setInterval(() => {
        if(rotationDuration > 0 && spinning)
        {
            rotation = (rotation+(Math.random()*10))%360
            console.log(rotation)
            rotationDuration = rotationDuration - Math.random()
        }
        else{
            spinning= false
            lowest=0
            highest=0
            rotationDuration=0
        }
    }, 10);

    function rotate(){
        spinning=true
        const duration = (Math.random()*5)+200
        highest=duration
        rotationDuration=duration
    }
</script>

<button on:click={() => rotate()}>spin</button>

<svg viewbox="0 0 {r*2+outerWidth*2} {r*2+outerWidth*2}" width={r+outerWidth*2} height={r+outerWidth*2} >
    <defs>
        <radialGradient id="myGradient">
          <stop offset="0%" stop-color="pink" />
          <stop offset="100%" stop-color="lightblue" />
        </radialGradient>
      </defs>
    <g transform="translate({r+outerWidth} {r+outerWidth}) rotate({rotation}) translate({-r} {-r})">
        <circle r={r} cx={r} cy={r} stroke-width={outerWidth} stroke="yellow" fill="url(#myGradient)" />
            {#if list?.length > 1}
                {#each Array(cntSlices) as angle, idx}
                    {@const x=Math.cos(sliceAngle*idx)}
                    {@const y=Math.sin(sliceAngle*idx)}
                    {@const x2=Math.cos(sliceAngle*(idx+1))}
                    {@const y2=Math.sin(sliceAngle*(idx+1))}
                    {@const tx=Math.cos(sliceAngle*(idx+.5))*contentRadius+r}
                    {@const ty=Math.sin(sliceAngle*(idx+.5))*contentRadius+r}
                    {@const contentRotation=90+(360/cntSlices*(idx+0.5))|0}
                    {@const path = [
                            `M ${x*r+r} ${y*r+r}`,
                            `A ${r} ${r} 0 0 1 ${x2*r+r} ${y2*r+r}`,
                            `L ${r} ${r}`,
                        ].join(' ')}
                    <path d={path} fill={colors[idx%colors.length]} class="wheel-slice" />
                    <g>
                        <text font-size={"1.5rem"} x={tx} y={ty} text-anchor ="middle" style="overflow: hidden; text-overflow: ellipsis; width: 80%; padding: 1rem;">{list[idx%list.length]}</text>
                    </g>
                {/each}
            {/if}
    </g>
</svg>

<style>
    .wheel-slice:hover{
        filter: drop-shadow(0px 0px 1rem black);
    }

    @keyframes rotate{
        0% {rotate: 0deg}
        100% {rotate: 360deg}
    }
</style>