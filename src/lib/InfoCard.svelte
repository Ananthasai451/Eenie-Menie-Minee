<div class="info-card-wrapper">
    <div class="info-card">
        <div class="card-front card">
            <div style="color: red;">What am I</div>
            <div style="margin-top: 0.5rem; font-size: 1.5rem;">
                Eenie, meenie, miney, moe,<br/>
                Catch a tiger by the toe.<br/>
                If he hollers, let him go,<br/>
                Eenie, meenie, miney, moe.
            </div>
        </div>
        <div class="card-back card">
            "Eenie, Meenie, Miney, Moe" is a traditional children's counting rhyme or game used to make a selection from a group of people.
        </div>
    </div>
</div>

<style>
    .info-card-wrapper{
        width: 25rem;
        height: 15rem;
        position: relative;
        margin-top: 2rem;
    }
    .info-card{
        width: 100%;
        height: 100%;
        font-size: 1rem;
        font-weight: 600;
        font-family: cursive;
        cursor: pointer;
        transition: 1s;
        position: relative;
        backface-visibility: hidden;
        -webkit-backface-visibility: hidden;
        transform-style: preserve-3d;
    }
    .info-card:hover{
        transform: rotateY(180deg);
    }
    .card-front{
        background-color: pink;
    }
    .card-back{
        transform: rotateY(180deg);
        background-color: rgb(249, 119, 141);
        font-size: 1rem;
        white-space: wrap;
    }
    .card{
        width: 100%;
        height: 100%;
        backface-visibility: hidden;
        -webkit-backface-visibility: hidden;
        position: absolute;
        top:0;
        left: 0;
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
        border-radius: 1rem;
        text-align: center;
    }
</style>