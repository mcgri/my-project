<script>
	import GridMasonry from "$lib/components/GridMasonry.svelte";
  import "../app.css";
</script>

<div class="h-screen w-screen relative">
<div class="absolute z-0">
  <GridMasonry /> 
</div>
<div class="absolute z-10 flex items-center justify-center w-full h-full">
<slot />
</div>

</div>
