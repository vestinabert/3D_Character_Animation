var createScene = function () {
    var scene = new BABYLON.Scene(engine);
    scene.createDefaultEnvironment();
    var camera = new BABYLON.ArcRotateCamera("Camera", 0, 0, 2, new BABYLON.Vector3(0, 1, 0), scene);
    camera.setPosition(new BABYLON.Vector3(2, 2, 2));
    camera.attachControl(canvas, true);
    var light = new BABYLON.HemisphericLight("light", new BABYLON.Vector3(0, 1, 0), scene);
    light.intensity = 0.6;

    // Keyboard events
    var inputMap = {};
    scene.actionManager = new BABYLON.ActionManager(scene);
    scene.actionManager.registerAction(new BABYLON.ExecuteCodeAction(BABYLON.ActionManager.OnKeyDownTrigger, function (evt){
        inputMap[evt.sourceEvent.key] = evt.sourceEvent.type == "keydown";
    }));
    scene.actionManager.registerAction(new BABYLON.ExecuteCodeAction(BABYLON.ActionManager.OnKeyUpTrigger, function (evt){
        inputMap[evt.sourceEvent.key] = evt.sourceEvent.type == "keydown";
    }));

    // GUI
    var advancedTexture = BABYLON.GUI.AdvancedDynamicTexture.CreateFullscreenUI("UI");
    var instructions = new BABYLON.GUI.TextBlock();
    instructions.text = "Move w/ WASD keys, K for a kick";
    instructions.color = "white";
    instructions.fontSize = 16;
    instructions.textHorizontalAlignment = BABYLON.GUI.Control.HORIZONTAL_ALIGNMENT_RIGHT
    instructions.textVerticalAlignment = BABYLON.GUI.Control.VERTICAL_ALIGNMENT_BOTTOM
    advancedTexture.addControl(instructions);

    BABYLON.SceneLoader.ImportMesh("", "https://dl.dropbox.com/s/bu6hr6xh5xuatve/", "mixamo2.glb", scene, function (meshes){          
        var hero = meshes[0];

        //Scale the model down        
        hero.scaling.scaleInPlace(0.5);

        //Lock camera on the character 
        camera.target = hero;

        //Character variables 
        var heroSpeed = 0.02;
        var heroSpeedBackwards = 0.005;
        var heroRotationSpeed = 0.1;

        var animating = true;

        const idleAnim = scene.getAnimationGroupByName("Idle");
        const walkAnim = scene.getAnimationGroupByName("Walking");
        const walkbackAnim = scene.getAnimationGroupByName("WalkingBack");
        const kickAnim = scene.getAnimationGroupByName("Kick");

        //Rendering loop (executed for everyframe)
        scene.onBeforeRenderObservable.add(() => {
            
            var keydown = false;
            if (inputMap["w"]) {
                hero.moveWithCollisions(hero.forward.scaleInPlace(heroSpeed));
                keydown = true;
            }
            if (inputMap["s"]) {
                hero.moveWithCollisions(hero.forward.scaleInPlace(-heroSpeedBackwards));
                keydown = true;
            }
            if (inputMap["a"]) {
                hero.rotate(BABYLON.Vector3.Up(), -heroRotationSpeed);
                keydown = true;
            }
            if (inputMap["d"]) {
                hero.rotate(BABYLON.Vector3.Up(), heroRotationSpeed);
                keydown = true;
            }
            if (inputMap["k"]) {
                keydown = true;
            }
 
            if (keydown) {
                if (!animating) {
                    animating = true;
                    if (inputMap["s"]) {
                        //Walk backwards
                        walkbackAnim.start(true, 1.0, walkbackAnim.from, walkbackAnim.to, false);
                    } else if
                        (inputMap["k"]) {
                        //Kick
                        kickAnim.start(true, 1.0, kickAnim.from, kickAnim.to, false);
                    }  else {
                        //Walk
                        walkAnim.start(true, 1.0, walkAnim.from, walkAnim.to, false);
                    }
                }
            } else {
                if (animating) {
                    //Default animation is idle when no key is down     
                    idleAnim.start(true, 1.0, idleAnim.from, idleAnim.to, false);

                    //Stop all animations besides Idle Anim when no key is down
                    walkAnim.stop();
                    walkbackAnim.stop();
                    kickAnim.stop();

                    //Ensure animation are played only once per rendering loop
                    animating = false;
                }
            }
        });

    });
    return scene;
}
