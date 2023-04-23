<script>
import JSZip from "jszip";
import { saveAs } from "file-saver";

//皮肤包结构
// |skin_pack
// |-|texts
// |-|-en_US.lang
// |-|-zh_CN.lang
// |-|-languages.json
// |-skin.png
// |-manifest.json
// |-skins.json

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
//根据图片路径转base64

var skin = {
    manifest: {
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
    },
    skins: {
        serialize_name: "包名",//包名
        localization_name: "TemplateSkinPack",//标识符
        skins: [
            {
                localization_name: "Skin1",//皮肤标识符
                geometry: "geometry.humanoid.customSlim",//皮肤模型细手臂
                texture: "skin1.png",
                type: "free",
            },
            {
                localization_name: "Skin2",//皮肤标识符
                geometry: "geometry.humanoid.custom",//皮肤模型粗手臂
                texture: "skin2.png",//皮肤图片路径
                type: "free",//皮肤是免费
            }
        ],
    },
    zh_CN: {
        "skinpack": "包名",
        "skin1.name": "皮肤1",
        "skin2.name": "皮肤2",
    },
    lang: (lang) => {
        var str = "skinpack.";
        str = str + skin.skins.localization_name + "=" + lang.skinpack + "\n";
        for (var i = 0; i < skin.skins.skins.length; i++) {
            str += 'skin.'+skin.skins.skins[i].localization_name + ".name=" + lang[skin.skins.skins[i].localization_name + ".name"] + "\n";
        }
        return str;
    }
};

var base64='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'

var zip = new JSZip();

zip.file("manifest.json", JSON.stringify(skin.manifest, null, 4));
zip.file("skins.json", JSON.stringify(skin.skins, null, 4));
var texts = zip.folder("texts");
texts.file("zh_CN.lang", skin.lang(skin.zh_CN));
zip.file("skin1.png", base64,{ base64: true });
zip.file("skin2.png", base64,{ base64: true });
zip.generateAsync({ type: "blob" })
    .then(function (content) {
        saveAs(content, "example.zip.mcpack");
    });
// zip.file("texts/en_US.lang", skin.lang(skin.en_US));

// var zip = new JSZip();
// zip.file("Hello.json", "{Hello:World}\n");
// var img = zip.folder("images");
// var imgData = "";
// imgData = imgData.split(",")[1];
// img.file("image.png", imgData, { base64: true });
// zip.generateAsync({ type: "blob" }).then(function (content) {
//   saveAs(content, "example.zip.mcpack");
// });
</script>

<style scoped></style>
<template></template>
