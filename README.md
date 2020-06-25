<!doctype html>
<html lang="en">

<head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <meta name="author" content="ZXing for JS">

    <title>Source of origin</title>

    <link rel="stylesheet" rel="preload" as="style" onload="this.rel='stylesheet';this.onload=null" href="https://fonts.googleapis.com/css?family=Roboto:300,300italic,700,700italic">
    <link rel="stylesheet" rel="preload" as="style" onload="this.rel='stylesheet';this.onload=null" href="normalize.css">
    <link rel="stylesheet" rel="preload" as="style" onload="this.rel='stylesheet';this.onload=null" href="milligram.min.css">
    <style>
        html,body{
            background-color: #000;
        }
        .scanner {
	width: 100%;
	height: 3px;
	background-color: red;
  opacity: 0.7;
  position: absolute;;
  box-shadow: 0px 0px 8px 10px rgba(170, 11, 23, 0.49);
  top:50%;
}
.scanner.anim{
    animation-name: scan;
  animation-duration: 4s;
  animation-timing-function: linear;
  animation-iteration-count: infinite;
  animation-play-state: running;
}
@keyframes scan {
  0% {
    box-shadow: 0px 0px 8px 10px rgba(170, 11, 23, 0.49);
    top: 50%;
  }  
  25% {
    box-shadow: 0px 6px 8px 10px rgba(170, 11, 23, 0.49);
    top: 5px;
  }
  75% {
    box-shadow: 0px -6px 8px 10px rgba(170, 11, 23, 0.49);
    top: 98%;
  }
}
.anim-box {
	position: relative;
	width: 100%;
	height: 252px;
	transition: transform .6s ease-out;
}
.anim-box:before, .anim-box:after, .anim-box>:first-child:before, .anim-box>:first-child:after {
	position: absolute;
	width: 10%; height: 15%;
	border-color: white; 
	border-style: solid;
	content: ' ';
}
/* top left corner */
.anim-box:before {
	top: 0; left: 0;
	border-width: 2px 0 0 2px;
}

/* top right corner */
.anim-box:after {
	top: 0; right: 0;
	border-width: 2px 2px 0 0;
}

/* bottom right corner */
.anim-box>:first-child:before {
	bottom: 0; right: 0;
	border-width: 0 2px 2px 0;
}

/* bottom left corner */
.anim-box>:first-child:after {
	bottom: 0; left: 0;
	border-width: 0 0 2px 2px;
}
.title{
    font-size: 22px;
    line-height: 1.2;
    color: #fff;
    text-align: center;

}
.buttonOuter{
    width: 100%;
    margin: 20px 0;
    text-align: center;
}
.buttonOuter .button{
    width: 45%;
}
pre>code {
    border-radius: 0;
    display: block;
    padding: 1rem 1.5rem;
    white-space: pre;
    /* height: 120px; */
    text-align: center;
    padding: 50px 0px;
    font-weight: bold;
}
#video{
border: 1px solid gray;
    position: initial;
    left: 0;
    top: 0;
    height: 100%;	    
}
.banner{
    font-size: 12px;
    margin-top: 20px;
    padding-top: 20px;
    display: inline-block;
}
    </style>
</head>

<body>

    <main class="wrapper" style="padding-top:2em">

        <section class="container" id="demo-content">
            <h1 class="title">
                Scan the barcode now to find out if your product is from India 🇮🇳
            </h1>

            

            

            <div class="anim-box" style="position: relative;">
                <div></div>
                <div class="scanner"></div>
                <video id="video" width="100%" height="100%"  style="border: 1px solid gray;pos"></video>
            </div>

            <div id="sourceSelectPanel" style="display:none">
                <label for="sourceSelect">Change video source:</label>
                <select id="sourceSelect" style="max-width:400px">
                </select>
            </div>
            <div class="buttonOuter">
                <a class="button" id="startButton">Scan Now</a>
                <a class="button" id="resetButton">Reset</a>
            </div>
            <label style="color: #fff;">Result:</label>
            <pre><code id="result"></code></pre>
	    <i class="banner">*The barcode doesn’t indicate where the product has actually been made. This website won’t tell if a product is ‘Made in India’ or ‘Made in China.’ It just shows where the product company is based in.</i>

        </section>

    </main>

    <script type="text/javascript" src="index.min.js"></script>
    <script
  src="https://code.jquery.com/jquery-3.5.1.min.js"
  integrity="sha256-9/aliU8dGd2tb6OSsuzixeV4y/faTqgFtohetphbbj0="
  crossorigin="anonymous"></script>
    <script type="text/javascript">
        window.addEventListener('load', function () {
            let selectedDeviceId;
            const codeReader = new ZXing.BrowserBarcodeReader()
            console.log('ZXing code reader initialized')
            codeReader.getVideoInputDevices()
                .then((videoInputDevices) => {
                    const sourceSelect = document.getElementById('sourceSelect')
                    selectedDeviceId = videoInputDevices[0].deviceId
                    if (videoInputDevices.length > 1) {
                        videoInputDevices.forEach((element) => {
                            const sourceOption = document.createElement('option')
                            sourceOption.text = element.label
                            sourceOption.value = element.deviceId
                            sourceSelect.appendChild(sourceOption)
                        })

                        sourceSelect.onchange = () => {
                            selectedDeviceId = sourceSelect.value;
                        }

                        const sourceSelectPanel = document.getElementById('sourceSelectPanel')
                        sourceSelectPanel.style.display = 'block'
                    }

                    document.getElementById('startButton').addEventListener('click', () => {
                        $('.scanner').addClass('anim');
                            $('.scanner').show();
                        codeReader.decodeOnceFromVideoDevice(selectedDeviceId, 'video').then((result) => {
                            var coo;
                            var barcode_size = parseInt(result.text.substring(0,3));
                            console.log(typeof(barcode_size));
                            if(barcode_size >= 000 && barcode_size <= 19){
                                coo = "U.S. and Canada";
                            }
                            if(barcode_size >= 20 && barcode_size <= 29){
                                coo = "Restricted distribution";
                            }
                            if(barcode_size >= 30 && barcode_size <= 39){
                                coo = "U.S. drugs";
                            }
                            if(barcode_size >= 40 && barcode_size <= 49){
                                coo = "Restricted distribution (MO defined)";
                            }  
                            if(barcode_size >= 50 && barcode_size <= 59){
                                coo = "Coupons";
                            }
                            if(barcode_size >= 60 && barcode_size <= 99){
                                coo = "U.S. and Canada";
                            }
                            if(barcode_size >= 100 && barcode_size <= 139){
                                coo = "U.S. and Canada";
                            } 
                            if(barcode_size >= 200 && barcode_size <= 299){
                                coo = "Restricted distribution";
                            } 
                            if(barcode_size >= 300 && barcode_size <= 379){
                                coo = "France and Monaco";
                            } 
                            if(barcode_size == 380){
                                coo = "Bulgaria";
                            } 
                            if(barcode_size == 383){
                                coo = "Slovenia";
                            }
                            if(barcode_size == 385){
                                coo = "Croatia";
                            } 
                            if(barcode_size == 387){
                                coo = "Bosnia and Herzegovina";
                            } 
                            if(barcode_size == 389){
                                coo = "Montenegro";
                            } 
                            if(barcode_size >= 400 && barcode_size <= 440){
                                coo = "Germany";
                            } 
                            if(barcode_size >= 450 && barcode_size <= 459){
                                coo = "Japan";
                            } 
                            if(barcode_size >= 460 && barcode_size <= 469){
                                coo = "Russia";
                            }
                            if(barcode_size == 470){
                                coo = "Kyrgyzstan";
                            } 
                            if(barcode_size == 471){
                                coo = "Taiwan";
                            }
                            if(barcode_size == 474){
                                coo = "Estonia";
                            }
                            if(barcode_size == 475){
                                coo = "Latvia";
                            }
                            if(barcode_size == 476){
                                coo = "Azerbaijan";
                            }
                            if(barcode_size == 477){
                                coo = "Lithuania";
                            }
                            if(barcode_size == 478){
                                coo = "Uzbekistan";
                            }
                            if(barcode_size == 479){
                                coo = "Sri Lanka";
                            }
                            if(barcode_size == 480){
                                coo = "Philippines";
                            }
                            if(barcode_size == 481){
                                coo = "Belarus";
                            }
                            if(barcode_size == 482){
                                coo = "Ukraine";
                            }
                            if(barcode_size == 484){
                                coo = "Moldova";
                            }
                            if(barcode_size == 485){
                                coo = "Armenia";
                            }
                            if(barcode_size == 486){
                                coo = "Georgia";
                            }
                            if(barcode_size == 487){
                                coo = "Kazakhstan";
                            }
                            if(barcode_size == 488){
                                coo = "Tajikistan";
                            }
                            if(barcode_size == 489){
                                coo = "Hong Kong SAR";
                            }
                            if(barcode_size >= 490 && barcode_size <= 499){
                                coo = "Japan";
                            }
                            if(barcode_size >= 500 && barcode_size <= 509){
                                coo = "United Kingdom";
                            }
                            if(barcode_size >= 520 && barcode_size <= 521){
                                coo = "Greece";
                            }
                            if(barcode_size == 528){
                                coo = "Lebanon";
                            }
                            if(barcode_size == 529){
                                coo = "Cyprus";
                            }
                            if(barcode_size == 530){
                                coo = "Albania";
                            }
                            if(barcode_size == 531){
                                coo = "Macedonia";
                            }
                            if(barcode_size == 535){
                                coo = "Malta";
                            }
                            if(barcode_size == 539){
                                coo = "Ireland";
                            }
                            if(barcode_size >= 540 && barcode_size <= 549){
                                coo = "Belgium and Luxembourg";
                            }
                            if(barcode_size == 560){
                                coo = "Portugal";
                            }
                            if(barcode_size == 569){
                                coo = "Iceland";
                            }
                            if(barcode_size >= 570 && barcode_size <= 579){
                                coo = "Denmark, Faroe Islands and Greenland";
                            }
                            if(barcode_size == 590){
                                coo = "Poland";
                            } 
                            if(barcode_size == 594){
                                coo = "Romania";
                            } 
                            if(barcode_size == 599){
                                coo = "Hungary";
                            } 
                            if(barcode_size >= 600 && barcode_size <= 601){
                                coo = "South Africa";
                            }
                            if(barcode_size == 603){
                                coo = "Ghana";
                            }
                            if(barcode_size == 604){
                                coo = "Senegal";
                            }
                            if(barcode_size == 608){
                                coo = "Bahrain";
                            }
                            if(barcode_size == 609){
                                coo = "Mauritius";
                            }
                            if(barcode_size == 611){
                                coo = "Morocco";
                            }
                            if(barcode_size == 613){
                                coo = "Algeria";
                            }
                            if(barcode_size == 615){
                                coo = "Nigeria";
                            }
                            if(barcode_size == 616){
                                coo = "Kenya";
                            }
                            if(barcode_size == 890){
                                coo = "India";
                            } 
                            if(barcode_size >= 690 && barcode_size <= 695){
                                coo = "China, The People’s Republic";
                            }
                            if(barcode_size >= 700 && barcode_size <= 709){
                                coo = "Norway";
                            }
                            if(barcode_size >= 730 && barcode_size <= 739){
                                coo = "Sweden : EAN/GS1 Sweden";
                            }
                            if(barcode_size >= 760 && barcode_size <= 769){
                                coo = "Sweden : EAN/GS1 Sweden";
                            }
                            if (barcode_size >= 770 && barcode_size <= 771) {
                                coo = "Colombia";
                            }
                            if (barcode_size >= 800 && barcode_size <= 839) {
                                coo = "Italy, San Marino and Vatican City";
                            }
                            if (barcode_size >= 840 && barcode_size <= 849) {
                                coo = "Spain and Andorra";
                            }
                            if (barcode_size >= 870 && barcode_size <= 879) {
                                coo = "Netherlands";
                            }
                            if (barcode_size >= 900 && barcode_size <= 919) {
                                coo = "Austria";
                            }
                            if (barcode_size >= 930 && barcode_size <= 939) {
                                coo = "Australia";
                            }
                            if (barcode_size >= 940 && barcode_size <= 949) {
                                coo = "New Zealand";
                            }
                            
                            document.getElementById('result').textContent = coo;
                            $('.scanner').removeClass('anim');
                            $('.scanner').hide();
                        }).catch((err) => {
                            console.error(err);
                            document.getElementById('result').textContent = err;
                            $('.scanner').removeClass('anim');
                            $('.scanner').show();
                        })
                        console.log(`Started continous decode from camera with id ${selectedDeviceId}`)
                    })

                    document.getElementById('resetButton').addEventListener('click', () => {
                        document.getElementById('result').textContent = '';
                        codeReader.reset();
                        console.log('Reset.');
                        $('.scanner').removeClass('anim');
                            $('.scanner').show();
                    })

                })
                .catch((err) => {
                    console.error(err)
                })
        })
    </script>

</body>

</html>
