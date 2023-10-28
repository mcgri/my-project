<script lang="ts">

import {Button} from "$lib/components/ui/button";
import Confetti from "svelte-confetti";
	import * as Card  from "$lib/components/ui/card";
	import { fade } from "svelte/transition";


  let count = 0;
  let posX = 0;
  let posY = 0;
let showConfetti = false;
let isMoving=false;
const buttonTexts = [
    {title:"Привет, Юльчик!",buttonText:"Привет, Даша... о_О"},
    {title:"Как дела??",buttonText:"Ну, нормально..."},
    {title:"У меня для тебя есть СЮРПРИЗ!",buttonText:"Надеюсь, на меня сейчас ничего не выскочит..."},
    {title:"Ты Готова?",buttonText:"Ну, давай. Я отодвинулась от экрана"},
    {title:"Точно готова??",buttonText:"Мне надо крикнуть как в Спанч Бобе: 'Да, Капитан!' ? :)"},

  ];

let original = {x:0,y:0};
function moveAway(event: MouseEvent ) {


    
    count += 1;
    setTimeout(() => {
        isMoving = false;
    }, 300);
}

</script>

{#if showConfetti}
  <div style="z-index: 1000; position: fixed; top: -50px; left: 0; height: 100vh; width: 100vw; display: flex; justify-content: center; overflow: hidden; pointer-events: none;">
    <Confetti x={[-5, 5]} y={[0, 0.1]} delay={[500, 2000]} infinite duration={5000} amount={500} fallDistance="100vh" />
  </div>
  
  <!-- Removed the 'opacity-0 scale-95' since you want it to appear immediately when 'showConfetti' is true -->

{/if}
  <div class={" flex flex-col font-bold text-center text-white drop-shadow-md transition-all duration-1000 ease-in transform dynamic-text-size "+ (showConfetti ? "scale-100 opacity-100 ":"hidden scale-0 opacity-0")}>
    <span>Ты станешь Тётей !!!</span>
    <span> 🤯🤪😅</span>
  </div>
    
{#if showConfetti===false}
<Card.Root class="w-fit max-w-xs max-h-screen overflow-auto " style={`transform: translate(${posX}px, ${posY}px); transition: transform 0.3s;`}>
  <Card.Header>
    <Card.Title class="text-4xl">{buttonTexts[count].title}</Card.Title>
    
  </Card.Header>

    <Card.Footer>
        {#if count<4}

        <Button class=" flex flex-wrap whitespace-normal max-w-xs h-fit text-lg" on:click={(e)=>{moveAway(e)}} >
     {buttonTexts[count].buttonText}
        </Button>

        {/if}
        {#if count===4 }

            <Button  class=" flex flex-wrap whitespace-normal max-w-xs h-fit text-lg" on:click={() => showConfetti = true}  >{buttonTexts[count].buttonText}</Button>
        
        {/if}

    </Card.Footer>
</Card.Root>


{/if}

