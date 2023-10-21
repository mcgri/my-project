<script lang="ts">
import * as Dialog from "$lib/components/ui/dialog";
import {Button} from "$lib/components/ui/button";
import Confetti from "svelte-confetti";

  let count = 0;
  let posX = 0;
  let posY = 0;
let showConfetti = false;
let isMoving=false;
  const buttonTexts = [
    "First Move",
    "Second Move",
    "Third Move",
    "Fourth Move",
    "Fifth Move",
    "Sixth Move",
    "Last Move"
  ];
// Utility function to get a random number between min and max.
function getRandomBetween(min: number, max: number): number {
    return Math.random() * (max - min) + min;
}

function moveAway(event?: MouseEvent | FocusEvent) {
    if (isMoving) return;

    if (count >= 7) {
        posX = 250;  // Reset to the center
        posY = 250;  // Reset to the center
       
        return;
    }
    isMoving = true;

    const rect = (event?.currentTarget as HTMLElement).getBoundingClientRect();

    let mouseY;

    if (event instanceof MouseEvent) {
        mouseY = event.clientY - rect.top;
    } else {
        // For a FocusEvent, use the center of the box as reference
        mouseY = 250;
    }

    if (count % 2 === 0) { // Even counts
        // Move to a random position on the right half of the box (between 250px to 500px)
        posX = getRandomBetween(250, 500);
    } else { // Odd counts
        // Move to a random position on the left half of the box (between 0px to 250px)
        posX = getRandomBetween(0, 250);
    }

    if (mouseY < 250) {
        // Mouse is on the top side, move button to the bottom
        posY = 500;
    } else {
        // Mouse is on the bottom side, move button to the top
        posY = 0;
    }

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
        <Confetti x={[-5, 5]} y={[0, 0.1]} delay={[500, 2000]} infinite duration={5000} amount={700} fallDistance="100vh" />
   
        </div>
{/if}


    <Dialog.Root>
        <div class="p-5 "  on:mouseover={moveAway} on:focus={moveAway} role="button" tabindex="0"  style={`transform: translate(${posX}px, ${posY}px); transition: transform 0.3s;`}>
          <Dialog.Trigger> 
            
            <Button  on:click={() => showConfetti = true} >{count < 7 ? buttonTexts[count] : "Button"}</Button>
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

