<template>
    <div class="div1">
        <canvas id="skin_container"></canvas>
    </div>
</template>
  
<script>
import { SkinViewer } from 'skinview3d';

// console.log( document.getElementById("skin_container"));

export default {
    name: 'Skin3d',
    mounted() {
        console.log(document.getElementById("skin_container"));
        let skinViewer = new SkinViewer({
            canvas: document.getElementById("skin_container"),
            width: 300,
            height: 400,
            skin: "src/assets/steve.png"
        });

    }
}







</script>
<style scoped>
#skin_container {
    width: 300px;
    height: 400px;
}

.div1 {
    width: 300px;
    height: 400px;
    margin: 0 auto;
}
</style>