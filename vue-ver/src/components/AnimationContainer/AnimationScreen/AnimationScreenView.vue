<template>
        <div class="viewer" :id="'viewer-'+screenId">
        </div>
</template>

<script>
import * as THREE from 'three/build/three.module.js'
import {FBXLoader} from 'three/examples/jsm/loaders/FBXLoader'
import { OrbitControls } from 'three/examples/jsm/controls/OrbitControls.js';

export default {
    name:"ScreenViewer",
    props:{
        screenId:{
            required:true
        },
        filename:{
            type:String,
            required:true
        }
    },
    data:{
        loader:null,
        scene:null,
    },
    mounted(){
            let self = this
            const parentsId = "viewer-"+self.screenId
            const filename = self.filename
            console.log(filename)
            var parents = document.getElementById(parentsId)
            console.log(parents)
            console.log(parentsId)
            console.log(parents.offsetWidth)
            console.log(parents.offsetHeight)
            var container, controls;
			var camera, scene, renderer, light;

			var clock = new THREE.Clock();

			var mixer;

			function onWindowResize() {

				camera.aspect =  parents.offsetWidth/ parents.offsetHeight;
				camera.updateProjectionMatrix();

				renderer.setSize( parents.offsetWidth ,parents.offsetHeight );

			}

			function init() {
                container = document.createElement( 'div' );
                parents.appendChild( container );

				camera = new THREE.PerspectiveCamera( 45, parents.offsetWidth / parents.offsetHeight, 1, 2000 );
				camera.position.set( 100, 200, 300 );

				scene = new THREE.Scene();
				scene.background = new THREE.Color( 0xa0a0a0 );
				scene.fog = new THREE.Fog( 0xa0a0a0, 200, 1000 );

				light = new THREE.HemisphereLight( 0xffffff, 0x444444 );
				light.position.set( 0, 200, 0 );
				scene.add( light );

				light = new THREE.DirectionalLight( 0xffffff );
				light.position.set( 0, 200, 100 );
				light.castShadow = true;
				light.shadow.camera.top = 180;
				light.shadow.camera.bottom = - 100;
				light.shadow.camera.left = - 120;
				light.shadow.camera.right = 120;
				scene.add( light );

				// scene.add( new CameraHelper( light.shadow.camera ) );

				// ground
				var mesh = new THREE.Mesh( new THREE.PlaneBufferGeometry( 2000, 2000 ), new THREE.MeshPhongMaterial( { color: 0x999999, depthWrite: false } ) );
				mesh.rotation.x = - Math.PI / 2;
				mesh.receiveShadow = true;
				scene.add( mesh );

				var grid = new THREE.GridHelper( 2000, 20, 0x000000, 0x000000 );
				grid.material.opacity = 0.2;
				grid.material.transparent = true;
				scene.add( grid );

				// model
				var loader = new FBXLoader();
				loader.load( filename, function ( object ) {

					mixer = new THREE.AnimationMixer( object );

					var action = mixer.clipAction( object.animations[ 0 ] );
					action.play();

					object.traverse( function ( child ) {

						if ( child.isMesh ) {

							child.castShadow = true;
							child.receiveShadow = true;

						}

					} );

					scene.add( object );

				} );

				renderer = new THREE.WebGLRenderer( { antialias: true } );
				renderer.setPixelRatio( window.devicePixelRatio );
				renderer.setSize( parents.offsetWidth, parents.offsetHeight );
				renderer.shadowMap.enabled = true;
				container.appendChild( renderer.domElement );

				controls = new OrbitControls( camera, renderer.domElement );
				controls.target.set( 0, 100, 0 );
				controls.update();

				window.addEventListener( 'resize', onWindowResize, false );
			}

			//

			function animate() {

				requestAnimationFrame( animate );

				var delta = clock.getDelta();

				if ( mixer ) mixer.update( delta );

				renderer.render( scene, camera );
            }
            init();
			animate();
    }
}
</script>

<style lang="scss" scoped>
.viewer{
    background-color:red;
    height:100%;
    width:100%;
}
</style>