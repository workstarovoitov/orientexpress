<!DOCTYPE html>
<html>
<head>
    <script src='src/phaser-arcade-physics.min.js'></script>
</head>
<body>

    <script>

    var Preloader = new Phaser.Class({

        Extends: Phaser.Scene,

        initialize:

        function Preloader ()
        {
            Phaser.Scene.call(this, 'preloader');
        },

        preload: function ()
        {
            this.load.image('imgMainScene', 'assets/background/scene1.0.jpg');
            this.load.image('imgMainSceneShelve', 'assets/background/scene1.1.jpg');
            this.load.image('imgMainSceneSwitches', 'assets/background/scene2.0.jpg');
            this.load.image('imgFin', 'assets/background/scene2.1.jpg');


            this.load.image('imgAC0', 'assets/bookAC/0.jpg');
            this.load.image('imgAC1', 'assets/bookAC/1.jpg');
            this.load.image('imgAC2', 'assets/bookAC/2.jpg');
            this.load.image('imgAC3', 'assets/bookAC/3.jpg');
            this.load.image('imgAC4', 'assets/bookAC/4.jpg');
            this.load.image('imgAC5', 'assets/bookAC/5.jpg');
            this.load.image('imgAC6', 'assets/bookAC/6.jpg');
            this.load.image('imgAC7', 'assets/bookAC/7.jpg');
            this.load.image('imgAC8', 'assets/bookAC/8.jpg');
            
            this.load.image('imgSM1', 'assets/serviceManual/1.jpg');
            this.load.image('imgSM2', 'assets/serviceManual/2.jpg');
            
            this.load.image('imgLampMain', 'assets/sprites/lampOnSceneMain.png');
            this.load.image('imgLampControl', 'assets/sprites/lampOnSceneControl.png');

            this.load.image('imgSwOnMain', 'assets/sprites/swOnSceneMain.png');
            this.load.image('imgSwOffMain', 'assets/sprites/swOffSceneMain.png');
            this.load.image('imgSwOnControl', 'assets/sprites/swOnSceneControl.png');
            this.load.image('imgSwOffControl', 'assets/sprites/swOffSceneControl.png');

            this.load.image('imgPinMain', 'assets/sprites/pinMain.png');
            this.load.image('imgPinControl', 'assets/sprites/pinControl.png');
            this.load.image('imgPinUnconnected', 'assets/sprites/pinUnconnected.png');

            this.load.image('imgLightOnControl', 'assets/sprites/onLightSceneControl.png');
            this.load.image('imgLightOffControl', 'assets/sprites/offLightSceneControl.png');

            this.load.image('imgLightOnMain', 'assets/sprites/onLightSceneMain.png');
            this.load.image('imgLightOffMain', 'assets/sprites/offLightSceneMain.png');
            this.load.image('imgStart', 'assets/sprites/start.png');
    
            this.load.audio('mp3SwOn', 'assets/sound/swOn.mp3');
            this.load.audio('mp3SwOff', 'assets/sound/swOff.mp3');
            this.load.audio('mp3SliderInstalled', 'assets/sound/sliderInstalled.mp3');
            this.load.audio('mp3SliderMove', 'assets/sound/sliderMove.mp3');
            this.load.audio('mp3PowerOn', 'assets/sound/powerOn.mp3');
            this.load.audio('mp3Bttn', 'assets/sound/bttn.mp3');
            this.load.audio('mp3Train', 'assets/sound/train.mp3');
            
        },

        create: function ()
        {
            this.scene.start('demoMain');
        }
        

    });

    var DemoMain = new Phaser.Class({

        Extends: Phaser.Scene,

        initialize:

        function DemoMain ()
        {
            Phaser.Scene.call(this, { key: 'demoMain', active: true });
        },
        preload: preloadMain,
        create: createMain,
        update: updateMain

    });

    var DemoBookAC = new Phaser.Class({

        Extends: Phaser.Scene,

        initialize:

        function DemoBookAC ()
        {
            Phaser.Scene.call(this, { key: 'demoBookAC', active: false });
        },
        preload: preloadBookAC,
        create: createBookAC

    });

    var DemoServiceManual= new Phaser.Class({

        Extends: Phaser.Scene,

        initialize:

        function DemoServiceManual ()
        {
            Phaser.Scene.call(this, { key: 'demoServiceManual', active: false });
        },
        preload: preloadBookAC,
        create: createServiceManual

    });

    var DemoMainSceneSwitches = new Phaser.Class({

        Extends: Phaser.Scene,

        initialize:

        function DemoMainSceneSwitches ()
        {
            Phaser.Scene.call(this, { key: 'demoMainSceneSwitches', active: false });
        },
       // preload: preloadMainSceneSwitches,
        create: createMainSceneSwitches,
        update: updateControl
    });


    var config = {
        type: Phaser.AUTO,
        backgroundColor: 'black',
        scale: {
        mode: Phaser.Scale.ENVELOP,
        parent: 'phaser-example',
        width: 1920,
        height: 1080,
        min: {
            width: 800,
            height: 600
        },
        max: {
            width: 1920,
            height: 1080
        }
        },
        scene: [Preloader,DemoMainSceneSwitches, DemoServiceManual, DemoBookAC, DemoMain],
        audio: { disableWebAudio: true }

    };
    var text1;
    var text2;
    var name;
    var fxDrawerOpen;
    var fxDrawerClose;
    var pageAC = 0;
    var pageSM;
    var manualOpenedFromMain = true;
    var switchesStates = [  false, false, false, false,
                            false, false, false, false,
                            false, false, false, false,
                            false, false, false, false];
    var switchesStatesWin = [  false, false, false, true,
                            true, false, true, true,
                            false, false, false, true,
                            true, false, false, true];

    var lampsMainXY = [ [197, 400], [294, 425], [288, 377], [258, 482],
                        [335, 480], [394, 464], [350, 517], [445, 498],
                        [392, 502], [502, 431], [487, 521], [553, 536],
                        [614, 570], [659, 496], [558, 453], [709, 582]];
    var switchesMainXY = [ [855, 278], [882, 278], [910, 278], [937, 278],
                        [855, 351], [882, 350], [910, 350], [937, 348],
                        [856, 427], [883, 425], [911, 423], [938, 421],
                        [856, 498], [883, 496], [911, 493], [938, 490]];
    var sliderPos = [2, 2, 2, 2];
    var sliderPosWin = [4, 2, 6, 5];
    var sliderEnable = [true, true, false, false];
    var sliderMainInstalled = false;
    var sliderControlInstalled = false;

    var slidersWin = false;
    var switchesWin = false;
    
    var sliderMainStartXY = [[920, 583], [919, 628], [1087, 558], [1087, 600]];
    var sliderMainEndXY = [[1030, 565], [1029, 608], [1184, 543], [1184, 581]];
    
    var sliderControlStartXY = [[1175, 642], [1175, 708], [1490, 642], [1490, 708]];
    var sliderControlEndXY = [[1388, 642], [1388, 708], [1704, 642], [1704, 708]];

    var lampsControlXY = [ [252, 329], [347, 364], [347, 312], [307, 423],
                        [387, 427], [454, 415], [400, 475], [510, 462],
                        [448, 459], [582, 388], [555, 494], [638, 520],
                        [713, 573], [779, 484], [648, 418], [848, 605]];
    var switchesControlXY = [ [1068, 213], [1119, 213], [1169, 213], [1214, 213],
                        [1068, 313], [1117, 313], [1166, 313], [1214, 313],
                        [1068, 413], [1117, 413], [1166, 413], [1214, 413],
                        [1068, 513], [1117, 513], [1166, 513], [1214, 513]];
    
    var test;
    var groupLampsMain;
    var groupSwitchesMain;
    var groupLampsControl;
    var groupSwitchesControl;
    var groupSliderMain;
    var groupSliderControl;
    var groupLightsMain;
    var groupLightsControl;
    var blinkEvent
    var pinMain;
    
    var game = new Phaser.Game(config);

    function preloadMain ()
    {
        this.load.audio('mp3DrawerOpen', 'assets/sound/drawerOpen.mp3');
        this.load.audio('mp3DrawerClose', 'assets/sound/drawerClose.mp3');
        this.load.audio('mp3PowerOn', 'assets/sound/powerOn.mp3');
    }

    function createMain ()
    {
        var powerOn = this.sound.add('mp3PowerOn');    
        var sliderInstalled = this.sound.add('mp3SliderInstalled');    
        
        imgOpened = this.add.image(0, 0, 'imgMainSceneShelve').setOrigin(0);
        imgClosed = this.add.image(0, 0, 'imgMainScene').setOrigin(0);
        imgOpened.visible = false;
        //this.add.image( 197, 400, 'imgLampMain').setScale(0.64);
        groupLampsMain = this.add.group();
        groupSwitchesMain = this.add.group();
        groupSliderMain = this.add.group();
        groupLightsMain = this.add.group();
        for (var i = 0; i < 16; i++) {
            groupLampsMain.create(lampsMainXY[i][0], lampsMainXY[i][1] , 'imgLampMain').setScale(0.64);
            groupSwitchesMain.create(switchesMainXY[i][0], switchesMainXY[i][1] , 'imgSwOffMain').setScale(0.64-(i%4)/50);
            groupSwitchesMain.create(switchesMainXY[i][0], switchesMainXY[i][1] , 'imgSwOnMain').setScale(0.64-(i%4)/50);
           //switchesMainSceneSw[i] = this.game.add.image( 0, 0, 'imgLampMain');
        }
        for (var i = 0; i < 4; i++) {
            groupSliderMain.create(Math.abs(sliderMainStartXY[i][0]-sliderMainEndXY[i][0])/7*sliderPos[i]+sliderMainStartXY[i][0], sliderMainStartXY[i][1] - Math.abs(sliderMainStartXY[i][1]-sliderMainEndXY[i][1])/7*sliderPos[i], 'imgPinMain').setOrigin().setScale(0.4);
        }
       
        for (var i = 0; i < 3; i++) {
            var child = groupLightsMain.create(1031 + i*44 , 292 - i, 'imgLightOnMain').setScale(0.64-i/100);
            child.visible = false;
            var child = groupLightsMain.create(1031 + i*45 , 292 - i, 'imgLightOffMain').setScale(0.64-i/100);
            child.visible = false;
        }


        
        var sprite_shelveClosed = this.add.sprite(0, 0); 
        var sprite_shelveOpened = this.add.sprite(0, 0);
        var sprite_books = this.add.sprite(0, 0);
        var sprite_controls = this.add.sprite(0, 0);
        var sprite_diary = this.add.sprite(0, 0);


        var zoneSlider3 = this.add.zone(0, 0).setOrigin(0);
        var zoneSlider4 = this.add.zone(0, 0).setOrigin(0);
        zoneSlider3.setName('zSlider3');
        zoneSlider4.setName('zSlider4');

        var plgn_shelveClosed = new Phaser.Geom.Polygon([1210, 900,1510, 800, 1510, 900,1210, 1010]);
        var plgn_books = new Phaser.Geom.Polygon([900, 690,1010, 700, 1000, 800,860, 800,845,745]);
        var plgn_controls = new Phaser.Geom.Polygon([830, 200,1230, 220, 1220, 690,830, 680]);
        var plgn_shelveOpened = new Phaser.Geom.Polygon([1210, 900,1510, 800, 1665, 870, 1665, 960, 1380, 1080, 1210,1010]);
        var plgn_diary = new Phaser.Geom.Polygon([1320, 880,1400, 875, 1440, 960, 1350, 960]);
        var plgnSlider3 = new Phaser.Geom.Polygon([1045, 515, 1215, 500, 1215, 545, 1045, 570]);
        var plgnSlider4 = new Phaser.Geom.Polygon([1045, 585, 1215, 560, 1215, 605, 1045, 640]);

        this.input.topOnly = true;
        
        sprite_shelveClosed.setInteractive(plgn_shelveClosed, Phaser.Geom.Polygon.Contains);
        sprite_books.setInteractive(plgn_books, Phaser.Geom.Polygon.Contains);
        sprite_controls.setInteractive(plgn_controls, Phaser.Geom.Polygon.Contains);
       
        zoneSlider3.setInteractive(plgnSlider3, Phaser.Geom.Polygon.Contains);
        zoneSlider4.setInteractive(plgnSlider4, Phaser.Geom.Polygon.Contains);
        zoneSlider3.input.dropZone = true;
        zoneSlider4.input.dropZone = true;
        
      
        sprite_shelveClosed.on('pointerdown', function (pointer, gameObject) {

            event.stopPropagation();
            this.sound.play('mp3DrawerOpen');
            imgClosed.visible = false;
            imgOpened.visible = true;
            if (!sliderMainInstalled){
                pinMain = this.add.sprite(1460, 910, 'imgPinUnconnected').setOrigin().setScale(0.35).setInteractive();
                this.input.setDraggable(pinMain);
            }
              
            sprite_shelveClosed.removeInteractive();
            sprite_shelveOpened.setInteractive(plgn_shelveOpened, Phaser.Geom.Polygon.Contains);
            sprite_diary.setInteractive(plgn_diary, Phaser.Geom.Polygon.Contains);

        },this);

        sprite_shelveOpened.on('pointerdown', function (pointer, gameObject) {

            event.stopPropagation();
            this.sound.play('mp3DrawerClose');
            imgClosed.visible = true;
            imgOpened.visible = false;

            if (!sliderMainInstalled){
                pinMain.destroy();
            }

            sprite_shelveClosed.setInteractive(plgn_shelveClosed, Phaser.Geom.Polygon.Contains);
            sprite_shelveOpened.removeInteractive();
            sprite_diary.removeInteractive();

       },this);

        sprite_books.on('pointerdown', function (pointer, gameObject) {
            manualOpenedFromMain = true;
            this.scene.switch('demoServiceManual');
        },this);

        sprite_controls.on('pointerdown', function (pointer, gameObject) {
            this.scene.switch('demoMainSceneSwitches');
        },this);

        zoneSlider3.on('pointerdown', function (pointer, gameObject) {
            this.scene.switch('demoMainSceneSwitches');
        },this);

        zoneSlider4.on('pointerdown', function (pointer, gameObject) {
            this.scene.switch('demoMainSceneSwitches');
        },this);


        sprite_diary.on('pointerdown', function (pointer, gameObject) {
            this.scene.switch('demoBookAC');
       },this);
    
        this.input.on('dragstart', function (pointer, gameObject) {

            this.children.bringToTop(gameObject);

        }, this);

        this.input.on('drag', function (pointer, gameObject, dragX, dragY) {
            gameObject.x = dragX;
            gameObject.y = dragY;
        });

        this.input.on('drop', function (pointer, gameObject, dropZone) {
            if (dropZone.name == 'zSlider3') {
                if(sliderEnable[2]){
                    gameObject.x = gameObject.input.dragStartX;
                    gameObject.y = gameObject.input.dragStartY;
                } else {
                    sliderMainInstalled = true;
                    sliderEnable[2] = true;
                    sliderInstalled.play();
                    if (sliderControlInstalled) powerOn.play();
                    gameObject.destroy();
                }
            }
            if (dropZone.name == 'zSlider4') {
                if(sliderEnable[3]){
                    gameObject.x = gameObject.input.dragStartX;
                    gameObject.y = gameObject.input.dragStartY;
                } else {
                    sliderMainInstalled = true;
                    sliderEnable[3] = true;
                    sliderInstalled.play();
                    if (sliderControlInstalled) powerOn.play();
                    gameObject.destroy();
                }
            }
            
            
            
            event.stopPropagation();
            //gameObject.input.enabled = false;

        });

        this.input.on('dragend', function (pointer, gameObject, dropped) {
            if (!dropped)
            {
                gameObject.x = gameObject.input.dragStartX;
                gameObject.y = gameObject.input.dragStartY;
            }
        });

       // text1 = this.add.text(10, 10, '', { fill: '#00ff00' });
    }

    function preloadBookAC ()
    {
        this.load.audio('mp3page', 'assets/sound/page.mp3');
    }

    function createBookAC ()
    {
        //this.add.image(0, 0, 'imgAC' + pageAC).setOrigin(0);    
        groupPagesAC = this.add.group();
        for (var i = 0; i <= 8; i++) {
            groupPagesAC.create(0, 0, 'imgAC' + i).setOrigin(0);
        }
        var childPagesAC = groupPagesAC.getChildren();
        for (var i = 0; i < 9; i++) {
            childPagesAC[i].visible = false;
        }
        childPagesAC[pageAC].visible = true;
        var sprite_background = this.add.sprite(0, 0);
        var sprite_leftPage = this.add.sprite(0, 0);
        var sprite_rightPage = this.add.sprite(0, 0);
       
        var plgn_background = new Phaser.Geom.Polygon([0, 0, 1920, 0, 1920, 1080, 0, 1080]);
        var plgn_leftPage = new Phaser.Geom.Polygon([300, 170, 880, 120, 940, 1020, 300, 1060]);
        var plgn_rightPage = new Phaser.Geom.Polygon([920, 115, 1490, 90, 1590, 965, 970, 1025]);

        this.input.topOnly = true;
        sprite_background.setInteractive(plgn_background, Phaser.Geom.Polygon.Contains);
        sprite_leftPage.setInteractive(plgn_leftPage, Phaser.Geom.Polygon.Contains);
        sprite_rightPage.setInteractive(plgn_rightPage, Phaser.Geom.Polygon.Contains);
        text2 = this.add.text(10, 10, '', { fill: '#00ff00' });
        sprite_leftPage.on('pointerdown', function (pointer, gameObject) {
            this.sound.play('mp3page');
           childPagesAC[pageAC].visible = false;
            if (pageAC > 0) {
                
                pageAC--;
            }
            childPagesAC[pageAC].visible = true;
        },this);

        sprite_rightPage.on('pointerdown', function (pointer, gameObject) {
            this.sound.play('mp3page');
            childPagesAC[pageAC].visible = false;
            if (pageAC < 8) {
                
                pageAC++;
            }
            childPagesAC[pageAC].visible = true;
        },this);
       
        sprite_background.on('pointerdown', function (pointer, gameObject) {
            this.scene.switch('demoMain');
       },this);

    }

    function createServiceManual ()
    {
        page2 = this.add.image(0, 0, 'imgSM2').setOrigin(0);  
        page1 = this.add.image(0, 0, 'imgSM1').setOrigin(0);  
        page2.visible = false;
        var firstPage = true;
        var sprite_background = this.add.zone(0, 0).setOrigin(0);
        var sprite_page = this.add.zone(0, 0).setOrigin(0);
        
        var plgn_background = new Phaser.Geom.Polygon([0, 0, 1920, 0, 1920, 1080, 0, 1080]);
        var plgn_page = new Phaser.Geom.Polygon([260, 40, 1660, 100, 1660, 1020, 200, 970]);
        
        this.input.topOnly = true;
        sprite_background.setInteractive(plgn_background, Phaser.Geom.Polygon.Contains);
        sprite_page.setInteractive(plgn_page, Phaser.Geom.Polygon.Contains);
       
        sprite_page.on('pointerdown', function (pointer, gameObject) {
            this.sound.play('mp3page');

            if (firstPage) {
                firstPage = false;
                page2.visible = true;
                page1.visible = false;
                //this.add.image(0, 0, 'imgSM2').setOrigin(0);
            }else{
                firstPage = true;
                page2.visible = false;
                page1.visible = true;
                
                //this.add.image(0, 0, 'imgSM1').setOrigin(0);
            }           
        },this);
       
        sprite_background.on('pointerdown', function (pointer, gameObject) {
            if (manualOpenedFromMain){
                this.scene.switch('demoMain');
            }else{
                this.scene.switch('demoMainSceneSwitches');
            }
            
       },this);

    }

    

    function createMainSceneSwitches ()
    {
        this.add.image(0, 0, 'imgMainSceneSwitches').setOrigin(0);    
        var imgFin = this.add.image(0, 0, 'imgFin').setOrigin(0).setInteractive();; 
        imgFin.visible = false;
       // text1 = this.add.text(500, 300, '', { fill: '#00ff00' });
       // text2 = this.add.text(500, 500, '', { fill: '#00ff00' });
        var fxOn = this.sound.add('mp3SwOn');
        var fxOff = this.sound.add('mp3SwOff');    
        var sliderMove = this.sound.add('mp3SliderMove');    
        var sliderInstalled = this.sound.add('mp3SliderInstalled');    
        var powerOn = this.sound.add('mp3PowerOn');   
        var soundBttn = this.sound.add('mp3Bttn');   
        var soundTrain = this.sound.add('mp3Train');    

        var zoneBackground = this.add.zone(0, 0).setOrigin(0); 
        var zoneBooks = this.add.zone(0, 0).setOrigin(0);
        var zoneSwitchesAll = this.add.zone(0, 0).setOrigin(0);
        
        var zoneSlider0 = this.add.zone(0, 0).setOrigin(0);
        var zoneSlider1 = this.add.zone(0, 0).setOrigin(0);
        var zoneSlider2 = this.add.zone(0, 0).setOrigin(0);
        var zoneSlider3 = this.add.zone(0, 0).setOrigin(0);
        zoneSlider0.setName('zSlider0');
        zoneSlider1.setName('zSlider1');
        zoneSlider2.setName('zSlider2');
        zoneSlider3.setName('zSlider3');

        zoneBackground.setName('zBackground');
        zoneBooks.setName('zBooks');
        zoneSwitchesAll.setName('zSwitchesAll');
       
        var plgnBackground = new Phaser.Geom.Polygon([0, 0, 1920, 0, 1920, 1080, 0, 1080]);
        var plgnBooks = new Phaser.Geom.Polygon([945, 825, 1125, 795, 1240, 820, 1065, 870]);
        var plgnSwitchesAll = new Phaser.Geom.Polygon([1030, 100, 1920, 100, 1920, 770, 1030, 770]);
        
        var plgnSlider0 = new Phaser.Geom.Polygon([1110, 605, 1410, 605, 1410, 655, 1110, 655]);
        var plgnSlider1 = new Phaser.Geom.Polygon([1110, 675, 1410, 675, 1410, 730, 1110, 730]);

        var plgnSlider2 = new Phaser.Geom.Polygon([1420, 605, 1730, 605, 1730, 655, 1420, 655]);
        var plgnSlider3 = new Phaser.Geom.Polygon([1420, 675, 1730, 675, 1730, 730, 1420, 730]);

        this.input.topOnly = true;
        
        zoneBackground.setInteractive(plgnBackground, Phaser.Geom.Polygon.Contains);
        zoneBooks.setInteractive(plgnBooks, Phaser.Geom.Polygon.Contains);
        zoneSwitchesAll.setInteractive(plgnSwitchesAll, Phaser.Geom.Polygon.Contains);
        
        zoneSlider0.setInteractive(plgnSlider0, Phaser.Geom.Polygon.Contains);
        zoneSlider1.setInteractive(plgnSlider1, Phaser.Geom.Polygon.Contains);
        zoneSlider2.setInteractive(plgnSlider2, Phaser.Geom.Polygon.Contains);
        zoneSlider3.setInteractive(plgnSlider3, Phaser.Geom.Polygon.Contains);
        
        zoneSlider0.input.dropZone = false;
        zoneSlider1.input.dropZone = false;
        zoneSlider2.input.dropZone = true;
        zoneSlider3.input.dropZone = true;

        zoneSwitchesAll.input.dropZone = false;
        zoneBooks.input.dropZone = false;
        zoneBackground.input.dropZone = false;

        var pin = this.add.sprite(1252, 882, 'imgPinUnconnected').setScale(0.4).setInteractive();
        var bttn = this.add.sprite(1716, 351, 'imgStart').setScale(1.05).setInteractive();
        this.input.setDraggable(pin);

        this.input.topOnly = true;
        
        groupLampsControl = this.add.group();
        groupSwitchesControl = this.add.group();
        groupSliderControl = this.add.group();
        groupLightsControl = this.add.group();
        for (var i = 0; i < 16; i++) {
            groupLampsControl.create(lampsControlXY[i][0], lampsControlXY[i][1] , 'imgLampControl').setScale(0.64);
            var swOffSprite = groupSwitchesControl.create(switchesControlXY[i][0], switchesControlXY[i][1] , 'imgSwOffControl');
            var swOnSprite = groupSwitchesControl.create(switchesControlXY[i][0], switchesControlXY[i][1] , 'imgSwOnControl');
            swOffSprite.name = i;
            swOnSprite.name = i;
        }

        for (var i = 0; i < 4; i++) {
            groupSliderControl.create(Math.abs(sliderControlStartXY[i][0]-sliderControlEndXY[i][0])/7*sliderPos[i]+sliderControlStartXY[i][0], sliderControlStartXY[i][1] - Math.abs(sliderControlStartXY[i][1]-sliderControlEndXY[i][1])/7*sliderPos[i], 'imgPinMain').setOrigin().setScale(0.75);
        }
        var childrenSlider = groupSliderControl.getChildren();
        //this.input.setDraggable(childrenSlider);

        for (var i = 0; i < 3; i++) {
            var child = groupLightsControl.create(1372 + i*86 , 225, 'imgLightOnControl').setScale(0.64);
            child.visible = false;
            var child = groupLightsControl.create(1372 + i*86 , 225, 'imgLightOffControl').setScale(0.64);
            child.visible = false;
        }


        groupSwitchesControl.inputEnableChildren = true;
        groupSliderControl.inputEnableChildren = true;


        //blinkEvent = this.time.addEvent({ delay: 500, callback: onBlink, callbackScope: this, repeat: 3 });


        this.input.on('dragstart', function (pointer, gameObject) {

            this.children.bringToTop(gameObject);

        }, this);

        this.input.on('drag', function (pointer, gameObject, dragX, dragY) {
            gameObject.x = dragX;
            gameObject.y = dragY;
        });

        this.input.on('drop', function (pointer, gameObject, dropZone) {
            if (dropZone.name == 'zSlider2') {
                if(sliderEnable[2]){
                    gameObject.x = gameObject.input.dragStartX;
                    gameObject.y = gameObject.input.dragStartY;
                } else {
                    sliderControlInstalled = true;
                    sliderEnable[2] = true;
                    sliderInstalled.play();
                    if (sliderMainInstalled) powerOn.play();
                    gameObject.destroy();
                }
            }
            if (dropZone.name == 'zSlider3') {
                if(sliderEnable[3]){
                    gameObject.x = gameObject.input.dragStartX;
                    gameObject.y = gameObject.input.dragStartY;
                } else {
                    sliderControlInstalled = true;
                    sliderEnable[3] = true;
                    sliderInstalled.play();
                    if (sliderMainInstalled) powerOn.play();
                    gameObject.destroy();
                }
            }
            
            
            
            event.stopPropagation();

        });

        this.input.on('dragend', function (pointer, gameObject, dropped) {
            if (!dropped)
            {
                gameObject.x = gameObject.input.dragStartX;
                gameObject.y = gameObject.input.dragStartY;
            }
        });

        
        this.input.setHitArea(groupSwitchesControl.getChildren()).on('gameobjectdown', function(pointer, gameObject) {
            event.stopPropagation();
            name = gameObject.name;
            switchesStates[gameObject.name] = !switchesStates[gameObject.name];
            if (name !== "" && name >= 0 && name <= 15){
                 if (switchesStates[gameObject.name] == true) fxOn.play();
                else fxOff.play();
            }
           
        });
        
        zoneBackground.on('pointerdown', function (pointer, gameObject) {
            //event.stopPropagation();
            this.scene.switch('demoMain');
        },this);

        zoneBooks.on('pointerdown', function (pointer, gameObject) {
            manualOpenedFromMain = false;
            this.scene.switch('demoServiceManual');
        },this);

        zoneSlider0.on('pointerdown', function (pointer, gameObject) {
            if (childrenSlider[0].x > pointer.worldX && sliderPos[0] > 0){
                sliderPos[0]--;
                sliderMove.play();
            }
            if (childrenSlider[0].x < pointer.worldX && sliderPos[0] < 7){
                sliderPos[0]++;
                sliderMove.play();
            }
        },this);

        zoneSlider1.on('pointerdown', function (pointer, gameObject) {
            if (childrenSlider[1].x > pointer.worldX && sliderPos[1] > 0){
                sliderPos[1]--;
                sliderMove.play();
            }
            if (childrenSlider[1].x < pointer.worldX && sliderPos[1] < 7){
                sliderPos[1]++;
                sliderMove.play();
            }
        },this);

        zoneSlider2.on('pointerdown', function (pointer, gameObject) {
            if (sliderEnable[2]){
                if (childrenSlider[2].x > pointer.worldX && sliderPos[2] > 0){
                    sliderPos[2]--;
                    sliderMove.play();
                }
                if (childrenSlider[2].x < pointer.worldX && sliderPos[2] < 7){
                    sliderPos[2]++;
                    sliderMove.play();
                }
            }
                
        },this);

        zoneSlider3.on('pointerdown', function (pointer, gameObject) {
            if (sliderEnable[3]){
                if (childrenSlider[3].x > pointer.worldX && sliderPos[3] > 0){
                    sliderPos[3]--;
                    sliderMove.play();
                }
                if (childrenSlider[3].x < pointer.worldX && sliderPos[3] < 7){
                    sliderPos[3]++;
                    sliderMove.play();
                }
            }
                
        },this);

        bttn.on('pointerdown', function (pointer, gameObject) {
            soundBttn.play();
            if (slidersWin && switchesWin){
                imgFin.setDepth(99);
                imgFin.visible = true;
                soundTrain.play();
            }
        },this);

    }

    function onBlink ()
    {
        var childrenLight = groupLightsControl.getChildren();
        if (lampNum == 2){
            childrenLight[4].visible = true;
        }
        if (lampNum == 1){
            if (sliderMainInstalled && sliderControlInstalled){
                var k = 0;
                for (var i = 0; i < 4; i++){
                    if (sliderPos[i] == sliderPosWin[i]) k++;
                }
                if (k == 4) {
                    childrenLight[2].visible = true;
                    childrenLight[3].visible = false;
                } else {
                    childrenLight[2].visible = false;
                    childrenLight[3].visible = true;
                }
            }
            
        }
        
        lampNum--;
    }

    function updateMain ()
    { 
        

        
        var childrenLamp = groupLampsMain.getChildren();
        var childrenSw = groupSwitchesMain.getChildren();
        var childrenSlider = groupSliderMain.getChildren();
        var childrenLight = groupLightsMain.getChildren();
        
        if (sliderMainInstalled && sliderControlInstalled){
            childrenLight[4].visible = true;

                
            var k = 0;
            for (var i = 0; i < 4; i++){
                if (sliderPos[i] == sliderPosWin[i]) k++;
            }
            if (k == 4) {
                slidersWin = true;
                childrenLight[2].visible = true;
                childrenLight[3].visible = false;

            } else {
                childrenLight[2].visible = false;
                childrenLight[3].visible = true;
                slidersWin = false;
            }

            var l = 0;
            for (var i = 0; i < 16; i++){
                if (switchesStates[i] == switchesStatesWin[i]) l++;
            }
            if (l == 16) {
                switchesWin = true;
                childrenLight[0].visible = true;
                childrenLight[1].visible = false;

            } else {
                childrenLight[0].visible = false;
                childrenLight[1].visible = true;
                switchesWin = false;
            }
        }

        for (var i = 0; i < 16; i++)
        {   if (!slidersWin) childrenLamp[i].visible = false;
            else childrenLamp[i].visible = switchesStates[i];
            childrenSw[i*2].visible = !switchesStates[i];
            childrenSw[i*2+1].visible = switchesStates[i];
        }

        for (var i = 0; i < 4; i++)
        {
            childrenSlider[i].visible = sliderEnable[i];
            childrenSlider[i].x = Math.abs(sliderMainStartXY[i][0]-sliderMainEndXY[i][0])/7*sliderPos[i]+sliderMainStartXY[i][0];
            childrenSlider[i].y = sliderMainStartXY[i][1] - Math.abs(sliderMainStartXY[i][1]-sliderMainEndXY[i][1])/7*sliderPos[i];
        }


    }

    function updateControl (){       
        var pointer = this.input.activePointer;

       // text1.setText([
       //     'x: ' + pointer.worldX,
       //     'y: ' + pointer.worldY,
        //    'isDown: ' + pointer.isDown
        //]);
        
        
        var childrenLamp1 = groupLampsControl.getChildren();
        var childrenSw1 = groupSwitchesControl.getChildren();
        var childrenSlider = groupSliderControl.getChildren();
        var childrenLight = groupLightsControl.getChildren();
        
        //childrenLight[4].visible = true;
        
        if (sliderMainInstalled && sliderControlInstalled){
            childrenLight[4].visible = true;

                
            var k = 0;
            for (var i = 0; i < 4; i++){
                if (sliderPos[i] == sliderPosWin[i]) k++;
            }
            if (k == 4) {
                slidersWin = true;
                childrenLight[2].visible = true;
                childrenLight[3].visible = false;

            } else {
                childrenLight[2].visible = false;
                childrenLight[3].visible = true;
                slidersWin = false;
            }

            var l = 0;
            for (var i = 0; i < 16; i++){
                if (switchesStates[i] == switchesStatesWin[i]) l++;
            }
            if (l == 16) {
                switchesWin = true;
                childrenLight[0].visible = true;
                childrenLight[1].visible = false;

            } else {
                childrenLight[0].visible = false;
                childrenLight[1].visible = true;
                switchesWin = false;
            }
            
        }

        for (var i = 0; i < 16; i++)
        {
            if (!slidersWin) childrenLamp1[i].visible = false;
            else childrenLamp1[i].visible = switchesStates[i];
            childrenSw1[i*2].visible = !switchesStates[i];
            childrenSw1[i*2+1].visible = switchesStates[i];
        }

        for (var i = 0; i < 4; i++)
        {
            childrenSlider[i].visible = sliderEnable[i];
            childrenSlider[i].x = Math.abs(sliderControlStartXY[i][0]-sliderControlEndXY[i][0])/7*sliderPos[i]+sliderControlStartXY[i][0];
            childrenSlider[i].y = sliderControlStartXY[i][1] - Math.abs(sliderControlStartXY[i][1]-sliderControlEndXY[i][1])/7*sliderPos[i];
        }

       
    }
    </script>

</body>
</html>
