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


  if (isMoving) return;

    if (count >= 4) {
        // Reset to the center after completing all movements
        posX = 250;
        posY = 250;
      
        return;
    }

    isMoving = true;

    let targetX, targetY;
      const rect = (event?.currentTarget as HTMLElement).getBoundingClientRect();

    // Get the mouse coordinates relative to the element
    const mouseX = event.clientX - rect.left;
    const mouseY = event.clientY - rect.top;
    if(original.x===0 && original.y===0){
      original.x=mouseX;
      original.y=mouseY;
    }
    
    switch (count) {
        case 0: // Move to the very right
            targetX =mouseX+100;
            targetY = mouseY;
            break;
        case 1: // Move to the very left
            targetX = mouseX-100;
            targetY = mouseY;
            break;
        case 2: // Move to the top
            targetX = mouseX-100;
            targetY = mouseY+100;
            break;
        case 3: // Move to the bottom
            targetX = mouseX-100;
            targetY = mouseY-100;
            break;
        case 4: // Move back to the center
            targetX =  original.x;
            targetY = original.y;
            break;
        default:
            break;
    }

        posX = targetX;
        posY = targetY;
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
<Card.Root class="w-fit" style={`transform: translate(${posX}px, ${posY}px); transition: transform 0.3s;`}>
  <Card.Header>
    <Card.Title class="text-4xl">{buttonTexts[count].title}</Card.Title>
    
  </Card.Header>

    <Card.Footer>
        {#if count<4}

        <Button class="w-full" on:click={(e)=>{moveAway(e)}} size="lg">
       {buttonTexts[count].buttonText}
        </Button>

        {/if}
        {#if count===4 }

            <Button  on:click={() => showConfetti = true}  size="lg">{buttonTexts[count].buttonText}</Button>
        
        {/if}

    </Card.Footer>
</Card.Root>


{/if}

