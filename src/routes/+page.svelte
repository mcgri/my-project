<script lang="ts">
import * as Dialog from "$lib/components/ui/dialog";
import {Button} from "$lib/components/ui/button";
import Confetti from "svelte-confetti";
	import * as Card  from "$lib/components/ui/card";

  let count = 0;
  let posX = 0;
  let posY = 0;
let showConfetti = false;
let isMoving=false;
  const buttonTexts = [
    {title:"Привет ЮЛЬЧИК!!!!!!",buttonText:"Привет Даша........ о_О"},
    {title:"Как дела?",buttonText:"Ну нормально."},
    {title:"А у меня для тебя СЮРПРИЗ!",buttonText:"Надеюсь на меня сейчас ничего не выскачит...."},
    {title:"Ты Готова?",buttonText:"Ну давай. Я отодвинулась от экрана"},
    {title:"Точно готова??",buttonText:"Мне что, надо крикнуть как в спанч бобе - ДА КАПИТАН??"},

  ];
// Utility function to get a random number between min and max.
function getRandomBetween(min: number, max: number): number {
    return Math.random() * (max - min) + min;
}


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
            targetX = mouseX-200;
            targetY = mouseY;
            break;
        case 2: // Move to the top
            targetX = mouseX;
            targetY = mouseY+100;
            break;
        case 3: // Move to the bottom
            targetX = mouseX;
            targetY = mouseY-200;
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
  <div style="
  z-index: 1000;
        position: fixed;
        top: -50px;
        left: 0;
        height: 100vh;
        width: 100vw;
        display: flex;
        justify-content: center;
        overflow: hidden;
        pointer-events: none;">
        <Confetti x={[-5, 5]} y={[0, 0.1]} delay={[500, 2000]} infinite duration={5000} amount={100} fallDistance="100vh" />
   
        </div>
{/if}


<Card.Root class="w-[480px]" style={`transform: translate(${posX}px, ${posY}px); transition: transform 0.3s;`}>
  <Card.Header>
    <Card.Title>{buttonTexts[count].title}</Card.Title>
    
  </Card.Header>

    <Card.Footer>
        {#if count<4}

        <Button class="w-full" on:click={(e)=>{moveAway(e)}}>
       {buttonTexts[count].buttonText}
        </Button>

        {/if}
        {#if count===4}
          <Dialog.Root>
        <div class=""  role="button" tabindex="0"  >
          <Dialog.Trigger> 
            
            <Button  on:click={() => showConfetti = true} >{buttonTexts[count].buttonText}</Button>
            </Dialog.Trigger>
        </div>
      
        <Dialog.Content>
            
            <Dialog.Header>
            <Dialog.Title>Are you sure absolutely sure?</Dialog.Title>
            <Dialog.Description>
                
                This action cannot be undone. This will permanently delete your account
                and remove your data from our servers.
            </Dialog.Description>
            </Dialog.Header>
        </Dialog.Content>
        </Dialog.Root>
        {/if}

    </Card.Footer>
</Card.Root>




