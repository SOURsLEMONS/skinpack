<script>
import { VueElement, ref } from "vue";
import { SkinViewer } from "skinview3d";
import JSZip from "jszip";
import { saveAs } from "file-saver";

var loaders = true

var steve =
    "data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAEAAAABACAMAAACdt4HsAAAAe1BMVEUAAACsdloAaGgAf38ApKQAr68AzMwElZUFiIgKvLwjIyMkGAgmGgomJCQoKCgrHg0zJBE0JRI6MYk/KhU/Pz9BIQxBNZtCHQpFIg5GOqVKSkpSKCZSPYlqQDBra2t1Ry+BUzmGUzSKTD2iakesdlqze2K9i3LGloD///+tRwrWAAAAAnRSTlMAAHaTzTgAAALLSURBVFjD7ZZhf5owEMZtTQSXdWRAmbBitTG03/8T7rm7xIrOEenLeYJJ+Hl/755E7xaLYEVhcdniNdjiVitshQuAft/v93MAcKbXa/8N1s+JAAngng8oJInZAEsaFlUA/J4BqDgHpMA2RwNLOt6+jbz5ht4rGAhYUDp0LDArUtQvjLHWEMHSyxg5VBU9r5Jy5xPk/DB495PXFW0oka2djoAixnfC/X0Agr0kDU4k6QAVzpnh42UYXj7ejXP8yBoG2ZT9t8YPHlffy2g4fkhBckwCTGEke//89rY5yBwPCVCkAJxz3nvndu7512aDQZZ48B22c0lnoYT9gD08yHolZ2lVBoufU1orCD1cBcT1So7zBUAzwLv5gHVqBFdS+GsETVM2bdvSQGOZkSl3ODjFU62h606TKQURljBafALKEaBlf5U5R+9CI9for5aPj0tanwDEcQTQSuf4MEeglAr+/MYAjFcBOQxxa9yZTMVzGewRRuMlABMSLAtu+GqacAIE8GKODZMxoCxb0kIAICBoSED+RwD9Ugc58360lQygK6YAAImAIPKgIYmQeQF4BuA62cZLDSAgAtAUS9gDBpARwI0jYEDTRBB/a65IfpaD9wBTjp8A/gzQdQ3n3jRd1223HUeQA6F1AFAyeTYIgAkjQA2/etvxgLuTzQAUUeQ0QRaYtALwAvAngKe6rp+6jgfcNWlBObVNnosuYCE1ElE0dIxY3O2/tLUY/7Joclfkbsn9wmRfcANAO/8lwFp/NQKVHIH0Cw2X+xJ/ptIPcIHVo7L+b0Dbhr6hpXIeOgQu80mA2HEcAerYoNwOiP1ANC5160+7BEhhOfYL3BN4LspcF6cj4LpWngBcqOxiaSlwyyAphIZgCIUxDVC2UvAJ4GJLEspzAuB4EgIg1HOfCDjvF6J/FGEScN4vxH4gyjAJOO8XpCEg+aQ/OP/8HwWygi0ive6eAAAAAElFTkSuQmCC";
var slemons =
    "data:image/png;base64,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";
var skinlist = [
    {
        name: "皮肤1",
        src: steve,
        slim: false,
    },
    // {
    //     name: "皮肤2",
    //     src: slemons,
    //     slim: false,
    // },
];

//生成uuid
function uuid() {
    var s = [];
    var hexDigits = "0123456789abcdef";
    for (var i = 0; i < 36; i++) {
        s[i] = hexDigits.substr(Math.floor(Math.random() * 0x10), 1);
    }
    s[14] = "4";
    s[19] = hexDigits.substr((s[19] & 0x3) | 0x8, 1);
    s[8] = s[13] = s[18] = s[23] = "-";

    var uuid = s.join("");
    return uuid;
}




var skins = {
    serialize_name: "生成的皮肤包",//包名
    localization_name: "SLSkinPack",//标识符
    skins: [
    ],
}


var zh_CN = {
    skinpack: "SL皮肤包",
    skinlist: [
        "皮肤1",
        "皮肤2",
    ],
}

function lang(lang) {
    var str = "skinpack.";
    str = str + skins.localization_name + "=" + lang.skinpack + "\n";
    for (var i = 0; i < skins.skins.length; i++) {
        str += 'skin.' + skins.localization_name + "." + skins.skins[i].localization_name + "=" + lang.skinlist[i].name + "\n";
        // //console.log(lang.skinlist[i]);
    }
    return str;
}


// var skinname = skinlist[skinlistindex].name;


// var skinmodel = false;

var skinViewer;


export default {
    name: "sk",
    data() {
        return {
            skinpackname: "SL皮肤包",
            ulr: steve,
            skinlist: skinlist,
            skinlistindex: 0,
            skinname: '',
            skinmodel: false,
            loaders: true
        };
    },
    methods: {
        //这是上传图片的方法
        upload: function (e) {
            var that = this;
            //console.log(e.target.files[0]);
            //获取图片大小
            let file = e.target.files[0];
            let reader = new FileReader();
            reader.readAsDataURL(file);
            let img = new Image();
            reader.onload = function (e) {
                //console.log(e.target.result);
                img.src = e.target.result;
                img.onload = function () {
                    //console.log(img.width);
                    //console.log(img.height);
                    if (
                        img.width != img.height && (img.width != 128 ||img.width != 64 || img.width != 32)
                    ) {
                        alert("图片尺寸不正确，请上传128 64 32的图片");
                        return;
                    }
                    //这里ulr
                    this.ulr = e.target.result;
                    //console.log(this.ulr);

                    skinViewer.loadSkin(this.ulr);
                    //console.log(skinViewer.loadSkin(this.ulr));
                    skinViewer.autoRotate = true;
                    setTimeout(() => {
                        skinViewer.autoRotate = false;
                    }, 6280);

                    // //console.log(skinViewer.playerObject.skin.modelType);

                    that.skinlistindex = that.skinlist.push({
                        name: "皮肤" + (skinlist.length + 1),
                        src: this.ulr,
                        slim: skinViewer.playerObject.skin.modelType,
                    }) - 1;

                    //调用skinmodelslim()方法
                    that.skinmodelslim(this.ulr);
                };
            };
        },
        //上一张 下一张
        skinlistbrowse: function (n) {
            if (this.skinlistindex + n >= 0 && this.skinlistindex + n < skinlist.length) {
                this.skinlist[this.skinlistindex].name = this.skinname;
                this.skinlistindex += n;
                this.skinname = this.skinlist[this.skinlistindex].name;
                //console.log(this.skinname);
            }


            if (skinlist[this.skinlistindex].slim) {
                //console.log(skinlist[this.skinlistindex].slim);
                skinViewer.loadSkin(skinlist[this.skinlistindex].src, { model: "slim" });
                this.skinmodel = true;
            } else {
                skinViewer.loadSkin(skinlist[this.skinlistindex].src, { model: "default" });
                this.skinmodel = false;
                //console.log(skinlist[this.skinlistindex].slim);
            }
            //console.log(skinlist[this.skinlistindex].slim);
            //console.log(this.skinlistindex);
            //console.log(this.skinmodel);

            //model
        },
        //切换模型 皮肤
        skinmodelslim(url = this.skinlist[this.skinlistindex].src) {
            if (this.skinmodel) {
                skinViewer.loadSkin(url, { model: "default" });
                this.skinlist[this.skinlistindex].slim = false;
                this.skinmodel = false;
            } else {
                skinViewer.loadSkin(url, { model: "slim" });
                //console.log(this.skinlistindex);
                //console.log(this.skinlist[this.skinlistindex]);
                this.skinlist[this.skinlistindex].slim = true;
                this.skinmodel = true;
            }
        },
        //删除皮肤
        skinlistdelete: function () {
            if (this.skinlist.length > 1) {
                this.skinlist.splice(this.skinlistindex, 1);
                this.skinlistindex = 0;
                this.skinname = this.skinlist[this.skinlistindex].name;
                if (this.skinlist[this.skinlistindex].slim) {
                    skinViewer.loadSkin(this.skinlist[this.skinlistindex].src, {
                        model: "slim",
                    });
                    this.skinmodel = true;
                } else {
                    skinViewer.loadSkin(this.skinlist[this.skinlistindex].src, {
                        model: "default",
                    });
                    this.skinmodel = false;
                }
            } else {
                alert("至少保留一张皮肤");
            }
        },
        //生成皮肤包
        spawnpack: function () {
            this.loaders = true;
            var that = this;
            this.skinlist[this.skinlistindex].name = this.skinname;
            // //console.log(this.skinlist);

            var manifest = {
                header: {
                    name: "包名",
                    version: [1, 0, 0],
                    uuid: uuid(),
                },
                modules: [
                    {
                        version: [1, 0, 0],
                        type: "skin_pack",
                        uuid: uuid(),
                    },
                ],
                format_version: 1,
            }


            var time = new Date();
            time = time.getTime().toString()
            time = time.substring(time.length, time.length - 6);

            skins.serialize_name = this.skinpackname;
            skins.localization_name = "SLSkinPack" + time;
            skins.skins = function () {
                var arr = [];
                for (var i = 0; i < that.skinlist.length; i++) {
                    arr.push({
                        localization_name: 'Skin' + i,
                        geometry: skinlist[i].slim ? "geometry.humanoid.customSlim" : "geometry.humanoid.custom",
                        texture: 'skin' + i + '.png',
                        type: 'free',
                    });
                }
                return arr;
            }();
            //console.log(JSON.stringify(skins, null, 4));

            manifest.header.name = this.skinpackname;
            //console.log(JSON.stringify(manifest, null, 4));

            zh_CN.skinpack = this.skinpackname;
            zh_CN.skinlist = function () {
                var arr = [];
                for (var i = 0; i < that.skinlist.length; i++) {
                    arr.push({
                        name: that.skinlist[i].name,
                    });
                }
                return arr;
            }();

            //console.log(lang(zh_CN));


            var zip = new JSZip();

            zip.file("manifest.json", JSON.stringify(manifest, null, 4));
            zip.file("skins.json", JSON.stringify(skins, null, 4));
            var texts = zip.folder("texts");
            texts.file("zh_CN.lang", lang(zh_CN));
            for (var i = 0; i < this.skinlist.length; i++) {
                zip.file("skin" + i + ".png", this.skinlist[i].src.split(",")[1], {
                    base64: true,
                });
            }
            zip.generateAsync({ type: "blob" })
                .then(function (content) {
                    saveAs(content, "SLSkinPack.zip.mcpack");
                    that.loaders = false;
                });
        },
    },
    mounted() {
        skinViewer = new SkinViewer({
            canvas: document.getElementById("skin_container"),
            width: 150,
            height: 200,
            skin: steve,
        });
        this.loaders = false;
        // //console.log(this);
        // this.skinmodel = false;
    },
};
// //console.log(Vue);
</script>
<template>
    <div class="loaders" v-if="loaders">
        <img src="../assets/ing.png">
        <p>正在工作</p>
    </div>
    <div class="up">
        <div id="upimg" class="mcbutton">
            <input class="upimg" type="file" @change="upload" accept=".png" style="opacity: 0" />
            <p>上传图片</p>
        </div>
    </div>
    <div class="skin">
        <div class="skinimg">
            <canvas id="skin_container"></canvas>
        </div>
        <div class="skinlist">
            <button class="previous" @click="skinlistbrowse(-1)">
                <!-- <svg viewBox="0 0 512 512" id="pixel-arrow-left-news"><path d="M96 288v64h64v64h64v64h64V288h192v-64H288V32h-64v64h-64v64H96v64H32v64h64z"></path></svg> -->
                <img class="arrow" src="../assets/arrow-left.png" alt="" />
            </button>
            <button class="del" @click="skinlistdelete()">
                <img class="arrow del" src="../assets/arrow-del.png" /></button>
            <!-- <div class="skinlistimg"></div> -->
            <button class="unext" @click="skinlistbrowse(+1)">
                <!-- <svg viewBox="0 0 512 512" id="pixel-arrow-right-news"><path d="M416 224v-64h-64V96h-64V32h-64v192H32v64h192v192h64v-64h64v-64h64v-64h64v-64h-64z"></path></svg> -->
                <img class="arrow" src="../assets/arrow-right.png" alt="" />
            </button>
        </div>
        <div class="skininfo">
            <div class="skinname input-container">
                <input type="text" required="" v-model="skinname" />
                <!-- :value="skinname" -->
                <label for="input" class="label">皮肤名称</label>
                <div class="underline"></div>
            </div>
            <div class="skinmode input-container" @click="skinmodelslim()">
                <img src="../assets/model-default.png" alt="" />
                <div class="skinmodel" :class="skinmodel ? 'slim' : 'default'">
                    <button class="default">默认</button>
                    <button class="slim">瘦身</button>
                </div>
                <img src="../assets/model-slim.png" alt="" />
            </div>
            <div class="skinname input-container">
                <input type="text" required="" v-model="skinpackname" />
                <!-- :value="skinname" -->
                <label for="input" class="label">皮肤包名</label>
                <div class="underline"></div>
            </div>
            <div class="skinbutton">
                <button id="upimg" class="mcbutton" @click="spawnpack()">生成皮肤包</button>
            </div>
        </div>
    </div>
</template>

<style scoped>
.loaders {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background-color: #ffffff81;
    z-index: 999;
    display: flex;
    justify-content: center;
    align-items: center;
}

.loaders img {
    width: 100px;
    height: 100px;
    animation: spin 1s linear infinite;
}

@keyframes spin {
    0% {
        transform: scale(0.9, 1);
    }

    50% {
        transform: scale(1, 0.9);
    }

    100% {
        transform: scale(0.9, 1);
    }
}

.skinmode .default .default {
    background-color: #606060 !important;
    color: #fff !important;
    top: -2px;
    height: 40px;
    box-shadow: 0 -4px rgb(0 0 0 / 50%) inset, 0 4px #ccc inset, -4px 0 #333 inset,
        4px 0 rgb(0 0 0 / 50%) inset;
}

.skinmode .slim .slim {
    background-color: #606060 !important;
    color: #fff !important;
    top: -2px;
    height: 40px;
    box-shadow: 0 -4px rgb(0 0 0 / 50%) inset, 0 4px #ccc inset, -4px 0 #333 inset,
        4px 0 rgb(0 0 0 / 50%) inset;
}

.skinmode .skinmodel {
    display: flex;
    flex-direction: row;
    justify-content: space-between;
    align-items: center;
    width: 200px;
    height: 40px;
    pointer-events: none;
    top: 0;
    left: 0;
    content: "";
    transition: 0.7s cubic-bezier(0, 0.8, 0.26, 0.99);
    z-index: 1;
    background-color: #606060 !important;
    box-shadow: 0 -4px rgb(0 0 0 / 50%) inset, 0 4px #ccc inset, -4px 0 #333 inset,
        4px 0 rgb(0 0 0 / 50%) inset;
}

.skinmode .skinmodel button {
    position: relative;
    width: 60px;
    height: 30px;
    margin: 0;
    padding: 0;
    border: none;
    outline: none;
    border-radius: 0;
    background-color: rgba(0, 0, 0, 0);
    font-size: 16px;
    font-weight: 600;
    cursor: pointer;
    transition: all 0.3s ease;
    z-index: 10;
}

.skinmode .skinmodel button::before {
    content: "";
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    /* background-color: #008542; */
    z-index: -1;
    transition: all 0.3s ease;
}



.skinmode {
    display: flex;
    flex-direction: row;
    justify-content: space-between;
    align-items: center;
    width: 100%;
    height: 40px;
    /* margin-top: 20px; */
}

.skinmode img {
    width: 40px;
    /* height: 40px; */
    image-rendering: pixelated;
    margin: 2px;
}

.arrow {
    width: 30px;
    /* height: 30px; */
    filter: drop-shadow(0 0 0.5rem #3b3b3b);
    image-rendering: pixelated;
    transition: all 0.1s;
}

.skinlist button:active .arrow {
    transform: scale(0.8);
    filter: drop-shadow(0 0 0.5rem #ffffff);
}

.skinlist .del .del {
    width: 45px;
    filter: drop-shadow(0 0 0.5rem #ffa8a8);
}

.skinlist {
    width: 100%;
    height: 100%;
    display: flex;
    flex-direction: row;
    justify-content: space-between;
}

.skinlist button {
    /* width: 30px; */
    /* height: 40px; */
    outline: none;
    border-color: #ffffff00;
    background-color: #ffffff00;
    color: #ffffff;
    border-radius: 0%;
    /* opacity: 0; */
}

.skinlist button svg {
    /* width: 30px; */
    left: 73%;
    top: 10%;
    height: 30px;
    fill: #ffffff;
}

.skinlist .skinlistimg {
    width: 100%;
    height: 100%;
    /* background-color: #3b3b3b; */
}

.input-container {
    position: relative;
    margin: 40px auto;
    width: 200px;
}

.input-container input[type="text"] {
    font-size: 20px;
    width: 100%;
    border: none;
    border-bottom: 2px solid #ccc;
    padding: 5px 0;
    background-color: transparent;
    outline: none;
}

.input-container .label {
    position: absolute;
    top: 0;
    left: 0;
    color: #ccc;
    transition: all 0.3s ease;
    pointer-events: none;
}

.input-container input[type="text"]:focus~.label,
.input-container input[type="text"]:valid~.label {
    top: -20px;
    font-size: 16px;
    color: #333;
}

.input-container .underline {
    position: absolute;
    bottom: 0;
    left: 0;
    height: 2px;
    width: 100%;
    background-color: #333;
    transform: scaleX(0);
    transition: all 0.3s ease;
}

.input-container input[type="text"]:focus~.underline,
.input-container input[type="text"]:valid~.underline {
    transform: scaleX(1);
}

* {
    font-family: unset;
}

#skin_container {
    filter: drop-shadow(0 0 10px #828282);
}

.up {
    width: 100%;
    height: 100%;
    background-color: #ffffff00;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
}

.upimg {
    opacity: 0;
    width: 80px;
    height: 30px;
}

#upimg {
    position: relative;
    background-color: #f7ca5d;
    box-shadow: #ccc 0 0 10px;
    outline: none;
}

.mcbutton * {
    position: absolute;
    top: 0;
    left: 50%;
    width: 80px;
    transform: translate(-50%, 0%);
}

.mcbutton .upimg {
    position: relative;
    z-index: 5;
    transform: translate(-50%, 0%) scale(2);
}

.mcbutton {
    padding: 10px 40px;
    font-size: 18px;
    background-color: #008542;
    color: #fff;
    text-shadow: 0 2px 0 rgb(0 0 0 / 25%);
    display: inline-flex;
    align-items: center;
    justify-content: center;
    position: relative;
    border: 0;
    z-index: 1;
    user-select: none;
    cursor: pointer;
    text-transform: uppercase;
    letter-spacing: 1px;
    white-space: unset;
    padding: 0.8rem 1.5rem;
    text-decoration: none;
    font-weight: 900;
    transition: all 0.7s cubic-bezier(0, 0.8, 0.26, 0.99);
}

.mcbutton:before {
    position: absolute;
    pointer-events: none;
    top: 0;
    left: 0;
    display: block;
    width: 100%;
    height: 100%;
    content: "";
    transition: 0.7s cubic-bezier(0, 0.8, 0.26, 0.99);
    z-index: -1;
    background-color: #008542 !important;
    box-shadow: 0 -4px rgb(21 108 0 / 50%) inset,
        0 4px rgb(100 253 31 / 99%) inset, -4px 0 rgb(100 253 31 / 50%) inset,
        4px 0 rgb(21 108 0 / 50%) inset;
}

.mcbutton:after {
    position: absolute;
    pointer-events: none;
    top: 0;
    left: 0;
    display: block;
    width: 100%;
    height: 100%;
    content: "";
    box-shadow: 0 4px 0 0 rgb(0 0 0 / 15%);
    transition: 0.7s cubic-bezier(0, 0.8, 0.26, 0.99);
}

.mcbutton:hover:before {
    box-shadow: 0 -4px rgb(0 0 0 / 50%) inset, 0 4px rgb(255 255 255 / 20%) inset,
        -4px 0 rgb(255 255 255 / 20%) inset, 4px 0 rgb(0 0 0 / 50%) inset;
}

.mcbutton:hover:after {
    box-shadow: 0 4px 0 0 rgb(0 0 0 / 15%);
}

.mcbutton:active {
    transform: translateY(4px);
}

.mcbutton:active:after {
    box-shadow: 0 0px 0 0 rgb(0 0 0 / 15%);
}
</style>
