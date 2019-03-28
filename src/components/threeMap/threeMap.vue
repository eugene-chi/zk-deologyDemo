<!--suppress ALL -->
<template>
  <div id="container">
  </div>
</template>
<script>
  import * as THREE from "three";
  import {OBJLoader, MTLLoader} from 'three-obj-mtl-loader';
  import {CSS2DRenderer, CSS2DObject} from 'three-css2drender';
  
  const OrbitControls = require('three-orbit-controls')(THREE);
  export default {
    name: "threeMap",
    data() {
      return {
        scene: '',
        labelRenderer: '',
        light: '',
        camera: '',
        controls: '',
        renderer: '',
        geometry: '',
        material: '',
        cube: '',
        fov: 60,
        biaozhudiv: '',
        img: '',
        biaozhuLabel: ''
        
      }
    },
    mounted() {
      this.init();
      this.testBox();
      this.animate();
    },
    
    methods: {
      init() {
        this.scene = new THREE.Scene();
        this.scene.add(new THREE.AmbientLight(0x999999));//环境光
        this.light = new THREE.DirectionalLight(0xdfebff, 0.45);//从正上方（不是位置）照射过来的平行光，0.45的强度
        this.light.position.set(50, 200, 100);
        this.light.position.multiplyScalar(0.3);
        this.scene.add(this.light);
        //初始化相机
        this.camera = new THREE.PerspectiveCamera(this.fov, window.innerWidth / window.innerHeight, 1, 1000);
        this.camera.position.set(10, 90, 65);
        this.camera.lookAt(this.scene.position);
        //初始化控制器
        this.controls = new OrbitControls(this.camera);
        this.controls.target.set(100, 0, 0);
        this.controls.minDistance = 200;
        this.controls.maxDistance = 1000;
        this.controls.maxPolarAngle = Math.PI / 3;
        this.controls.update();
        //渲染
        this.renderer = new THREE.WebGLRenderer({
          alpha: true,
        });//会在body里面生成一个canvas标签,
        this.renderer.setPixelRatio(window.devicePixelRatio);//为了兼容高清屏幕
        this.renderer.setSize(window.innerWidth, window.innerHeight);
        
        const container = document.getElementById('container');
        container.appendChild(this.renderer.domElement);
        //标注渲染
        this.labelRenderer = new CSS2DRenderer();
        this.labelRenderer.setSize(window.innerWidth, window.innerHeight);
        this.labelRenderer.domElement.style.position = 'absolute';
        this.labelRenderer.domElement.style.top = 0;
        container.appendChild(this.labelRenderer.domElement);
        window.addEventListener('resize', this.onWindowResize, false);//添加窗口监听事件（resize-onresize即窗口或框架被重新调整大小）
      },
      onWindowResize() {
        this.camera.aspect = window.innerWidth / window.innerHeight;
        this.camera.updateProjectionMatrix();
        this.renderer.setSize(window.innerWidth, window.innerHeight);
        this.labelRenderer.setSize(window.innerWidth, window.innerHeight);
      },
      animate() {
        requestAnimationFrame(this.animate);
        this.render();
      },
      render() {
        this.renderer.render(this.scene, this.camera);
        this.labelRenderer.render(this.scene, this.camera);
      },
      testBox() {
        
        let group1 = new THREE.Group();
        
        let box1 = new THREE.BoxGeometry(2, 30, 40);
        let matColor1 = new THREE.MeshLambertMaterial({
          color: 0xfaa288,
          shading: THREE.FlatShading
        });
        let piece1 = new THREE.Mesh(box1, matColor1);
        let la1 = [];
        for (let i = 1; i < 30; i++) {
          la1[i] = piece1.clone();
          la1[i].position.x = 2 * i;
          la1[i].position.y = -i;
          la1[i].position.z = 28;
        }
        for (let i = 30; i < 70; i++) {
          la1[i] = piece1.clone();
          la1[i].position.x = 2 * i;
          la1[i].position.y = -30;
          la1[i].position.z = 28;
        }
        for (let i = 70; i < 100; i++) {
          la1[i] = piece1.clone();
          la1[i].position.x = 2 * i;
          la1[i].position.y = i - 100;
          la1[i].position.z = 28;
        }
        group1.add(piece1);
        for (let i = 1; i < 100; i++) {
          group1.add(la1[i]);
        }
        
        let box2 = new THREE.BoxGeometry(2, 50, 2);
        let matColor2 = new THREE.MeshLambertMaterial({
          color: 0xfaa200,
          shading: THREE.FlatShading
        });
        let piece2 = new THREE.Mesh(box2, matColor2);
        let la2 = [];
        for (let i = 1; i < 30; i++) {
          la2[i] = piece2.clone();
          la2[i].position.x = 2 * i;
          la2[i].position.y = -i;
          la2[i].position.z = 2;
        }
        for (let i = 30; i < 70; i++) {
          la2[i] = piece2.clone();
          la2[i].position.x = 2 * i;
          la2[i].position.y = -30;
          la2[i].position.z = 2;
        }
        for (let i = 70; i < 100; i++) {
          la2[i] = piece2.clone();
          la2[i].position.x = 2 * i;
          la2[i].position.y = i - 100;
          la2[i].position.z = 2;
        }
        group1.add(piece2);
        for (let i = 1; i < 100; i++) {
          group1.add(la2[i]);
        }
        
        
        let la3 = [];
        for (let i = 1; i < 30; i++) {
          la3[i] = piece2.clone();
          la3[i].position.x = 2 * i;
          la3[i].position.y = -i - 1;
          la3[i].position.z = 4;
        }
        for (let i = 30; i < 70; i++) {
          la3[i] = piece2.clone();
          la3[i].position.x = 2 * i;
          la3[i].position.y = -30 - 1;
          la3[i].position.z = 4;
        }
        for (let i = 70; i < 100; i++) {
          la3[i] = piece2.clone();
          la3[i].position.x = 2 * i;
          la3[i].position.y = i - 100 - 1;
          la3[i].position.z = 4;
        }
        for (let i = 1; i < 100; i++) {
          group1.add(la3[i]);
        }
        
        let la4 = [];
        for (let i = 1; i < 30; i++) {
          la4[i] = piece2.clone();
          la4[i].position.x = 2 * i;
          la4[i].position.y = -i - 2;
          la4[i].position.z = 6;
        }
        for (let i = 30; i < 70; i++) {
          la4[i] = piece2.clone();
          la4[i].position.x = 2 * i;
          la4[i].position.y = -30 - 2;
          la4[i].position.z = 6;
        }
        for (let i = 70; i < 100; i++) {
          la4[i] = piece2.clone();
          la4[i].position.x = 2 * i;
          la4[i].position.y = i - 100 - 2;
          la4[i].position.z = 6;
        }
        for (let i = 1; i < 100; i++) {
          group1.add(la4[i]);
        }
        
        this.scene.add(group1);//网格模型添加到场景中
  
        this.addSprite(50, -10, 55, '/static/image/sensor.png', -95, '传感器节点', () => {
          this.alarmDetail();
        });
      },
      addSprite(x, y, z, image, loc, text, callback) {
        //添加div标签
        this.biaozhudiv = document.createElement('div');
        //添加图标标签
        this.img = document.createElement('img');
        this.img.src = image;
        this.img.style.marginLeft = loc + 'px';
        this.biaozhudiv.className = 'lable';
        //两者的执行顺序
        this.biaozhudiv.textContent = text;
        this.biaozhudiv.appendChild(this.img);
        //标注的样式
        this.biaozhudiv.id = 'biaozhu';
        this.biaozhudiv.style.color = 'rgb(' + 0 + ',' + 0 + ',' + 0 + ')';
        this.biaozhudiv.style.fontSize = 15 + 'px';
        this.biaozhudiv.style.fontFamily = 'Georgia,serif';
        this.biaozhudiv.style.cursor = 'pointer';
        this.biaozhudiv.onclick = function () {
          callback();
        };
        this.biaozhuLabel = new CSS2DObject(this.biaozhudiv);
        this.biaozhuLabel.position.set(x, y, z);
        this.scene.add(this.biaozhuLabel);
      },
      //传感器详情界面
      alarmDetail() {
        this.$router.push('alarmPage');
        console.log("跳转到传感器详情界面");
      },
    }
  }
</script>

<style scoped>

</style>
