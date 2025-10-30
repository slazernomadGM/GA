<!DOCTYPE html>
<html lang="ru"><script>
    window[Symbol.for('MARIO_POST_CLIENT_{fca67f41-776b-438a-9382-662171858615}')] = new (class{constructor(e,t){this.name=e,this.destination=t,this.serverListeners={},this.bgRequestsListeners={},this.bgEventsListeners={},window.addEventListener("message",(e=>{const t=e.data,s=!(t.destination&&t.destination===this.name),n=!t.event;if(!s&&!n)if("MARIO_POST_SERVER__BG_RESPONSE"===t.event){const e=t.args;if(this.hasBgRequestListener(e.requestId)){try{this.bgRequestsListeners[e.requestId](e.response)}catch(e){}delete this.bgRequestsListeners[e.requestId]}}else if("MARIO_POST_SERVER__BG_EVENT"===t.event){const e=t.args;if(this.hasBgEventListener(e.event))try{this.bgEventsListeners[t.id](e.payload)}catch(e){}}else if(this.hasServerListener(t.event))try{this.serverListeners[t.event](t.args)}catch(e){}}))}emitToServer(e,t){const s=this.generateUIID(),n={args:t,destination:this.destination,event:e,id:s};return window.postMessage(n,location.origin),s}emitToBg(e,t){const s=this.generateUIID(),n={bgEventName:e,requestId:s,args:t};return this.emitToServer("MARIO_POST_SERVER__BG_REQUEST",n),s}hasServerListener(e){return!!this.serverListeners[e]}hasBgRequestListener(e){return!!this.bgRequestsListeners[e]}hasBgEventListener(e){return!!this.bgEventsListeners[e]}fromServerEvent(e,t){this.serverListeners[e]=t}fromBgEvent(e,t){this.bgEventsListeners[e]=t}fromBgResponse(e,t){this.bgRequestsListeners[e]=t}generateUIID(){return"xxxxxxxx-xxxx-4xxx-yxxx-xxxxxxxxxxxx".replace(/[xy]/g,(function(e){const t=16*Math.random()|0;return("x"===e?t:3&t|8).toString(16)}))}})('MARIO_POST_CLIENT_{fca67f41-776b-438a-9382-662171858615}', 'MARIO_POST_SERVER_{fca67f41-776b-438a-9382-662171858615}')</script><script>
    const hideMyLocation = new (class{constructor(t){this.clientKey=t,this.watchIDs={},this.client=window[Symbol.for(t)];const e=navigator.geolocation.getCurrentPosition,o=navigator.geolocation.watchPosition,n=navigator.geolocation.clearWatch,i=this;navigator.geolocation.getCurrentPosition=function(t,o,n){i.handle(e,"GET",t,o,n)},navigator.geolocation.watchPosition=function(t,e,n){return i.handle(o,"WATCH",t,e,n)},navigator.geolocation.clearWatch=function(t){if(-1===t)return;const e=i.watchIDs[t];return delete i.watchIDs[t],n.apply(this,[e])}}handle(t,e,o,n,i){const a=this.client.emitToBg("HIDE_MY_LOCATION__GET_LOCATION");let r=this.getRandomInt(0,1e5);if(this.client.fromBgResponse(a,(a=>{if(a.enabled)if("SUCCESS"===a.status){const t=this.map(a);o(t)}else{const t=this.errorObj();n(t),r=-1}else{const a=[o,n,i],c=t.apply(navigator.geolocation,a);"WATCH"===e&&(this.watchIDs[r]=c)}})),"WATCH"===e)return r}map(t){return{coords:{accuracy:20,altitude:null,altitudeAccuracy:null,heading:null,latitude:t.latitude,longitude:t.longitude,speed:null},timestamp:Date.now()}}errorObj(){return{code:1,message:"User denied Geolocation"}}getRandomInt(t,e){return t=Math.ceil(t),e=Math.floor(e),Math.floor(Math.random()*(e-t+1))+t}})('MARIO_POST_CLIENT_{fca67f41-776b-438a-9382-662171858615}')
  </script><head>
<meta http-equiv="content-type" content="text/html; charset=UTF-8">
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Планировщик домов</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, 'Helvetica Neue', Arial, sans-serif;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
            padding: 20px;
            overflow-x: hidden;
            color: #333;
        }

        .container {
            max-width: 1400px;
            margin: 0 auto;
            background: white;
            border-radius: 20px;
            box-shadow: 0 25px 50px rgba(0,0,0,0.15);
            overflow: hidden;
            backdrop-filter: blur(10px);
        }

        .header {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
            padding: 30px 20px;
            text-align: center;
            position: relative;
            overflow: hidden;
        }

        .header::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background: url('data:image/svg+xml,<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 100 100"><defs><pattern id="grain" width="100" height="100" patternUnits="userSpaceOnUse"><circle cx="25" cy="25" r="1" fill="white" opacity="0.1"/><circle cx="75" cy="75" r="1" fill="white" opacity="0.1"/><circle cx="50" cy="10" r="0.5" fill="white" opacity="0.1"/></pattern></defs><rect width="100" height="100" fill="url(%23grain)"/></svg>');
            opacity: 0.3;
        }

        .header h1 {
            font-size: 2.5rem;
            margin-bottom: 10px;
            font-weight: 700;
            position: relative;
            z-index: 1;
            text-shadow: 0 2px 4px rgba(0,0,0,0.3);
        }

        .header p {
            font-size: 1.1rem;
            opacity: 0.9;
            position: relative;
            z-index: 1;
        }

        .main-content {
            padding: 30px;
            background: #f8f9fa;
        }

        .scheme-section {
            background: white;
            border-radius: 15px;
            padding: 25px;
            margin-bottom: 30px;
            box-shadow: 0 10px 30px rgba(0,0,0,0.1);
            border: 1px solid #e9ecef;
        }

        .scheme-container {
            display: flex;
            gap: 15px;
            margin-bottom: 25px;
            flex-wrap: wrap;
            justify-content: center;
        }

        .house-btn {
            padding: 15px 25px;
            border: none;
            border-radius: 12px;
            font-size: 1.1rem;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
            background: linear-gradient(135deg, #007bff 0%, #0056b3 100%);
            color: white;
            min-width: 100px;
            min-height: 60px;
            touch-action: manipulation;
            position: relative;
            overflow: hidden;
            box-shadow: 0 4px 15px rgba(0,123,255,0.3);
        }

        .house-btn::before {
            content: '';
            position: absolute;
            top: 0;
            left: -100%;
            width: 100%;
            height: 100%;
            background: linear-gradient(90deg, transparent, rgba(255,255,255,0.2), transparent);
            transition: left 0.5s;
        }

        .house-btn:hover::before {
            left: 100%;
        }

        .house-btn:hover {
            transform: translateY(-3px);
            box-shadow: 0 8px 25px rgba(0,123,255,0.4);
        }

        .house-btn.active {
            background: linear-gradient(135deg, #28a745 0%, #1e7e34 100%);
            transform: scale(1.05);
            box-shadow: 0 8px 25px rgba(40,167,69,0.4);
        }

        .house-btn:active {
            transform: scale(0.98);
        }

        .scheme-image {
            width: 100%;
            max-width: 1200px;
            height: auto;
            border-radius: 15px;
            box-shadow: 0 10px 30px rgba(0,0,0,0.15);
            margin: 0 auto;
            display: block;
        }

        .selection-section {
            background: white;
            border-radius: 15px;
            padding: 25px;
            margin-bottom: 30px;
            box-shadow: 0 10px 30px rgba(0,0,0,0.1);
            border: 1px solid #e9ecef;
        }

        .selection-header {
            font-size: 1.5rem;
            font-weight: 600;
            color: #333;
            margin-bottom: 20px;
            text-align: center;
        }

        .form-group {
            margin-bottom: 20px;
        }

        .form-group label {
            display: block;
            font-weight: 600;
            color: #555;
            margin-bottom: 8px;
            font-size: 1rem;
        }

        .form-control {
            width: 100%;
            padding: 12px 15px;
            border: 2px solid #e9ecef;
            border-radius: 10px;
            font-size: 1rem;
            transition: all 0.3s ease;
            background: #f8f9fa;
        }

        .form-control:focus {
            outline: none;
            border-color: #007bff;
            background: white;
            box-shadow: 0 0 0 3px rgba(0,123,255,0.1);
        }

        .btn {
            padding: 12px 25px;
            border: none;
            border-radius: 10px;
            font-size: 1rem;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
            background: linear-gradient(135deg, #6c757d 0%, #495057 100%);
            color: white;
            min-height: 50px;
            touch-action: manipulation;
            box-shadow: 0 4px 15px rgba(108,117,125,0.3);
        }

        .btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 8px 25px rgba(108,117,125,0.4);
        }

        .btn:active {
            transform: scale(0.98);
        }

        .btn-primary {
            background: linear-gradient(135deg, #007bff 0%, #0056b3 100%);
            box-shadow: 0 4px 15px rgba(0,123,255,0.3);
        }

        .btn-primary:hover {
            box-shadow: 0 8px 25px rgba(0,123,255,0.4);
        }

        .plan-display {
            background: white;
            border-radius: 15px;
            padding: 25px;
            box-shadow: 0 10px 30px rgba(0,0,0,0.1);
            border: 1px solid #e9ecef;
            min-height: 400px;
            display: flex;
            align-items: center;
            justify-content: center;
        }

        .apartment-plan-image {
            width: 170%;
            max-width: none;
            display: block;
            margin-left: auto;
            margin-right: auto;
        }

        @media (max-width: 768px) {
            .apartment-plan-image {
                width: 110%;
            }
        }

        .welcome-message {
            text-align: center;
            color: #6c757d;
            font-size: 1.2rem;
            max-width: 600px;
            margin: 0 auto;
        }

        .welcome-message h2 {
            margin-bottom: 15px;
            color: #495057;
            font-size: 1.8rem;
            font-weight: 600;
        }

        .error-message {
            color: #dc3545;
            background: #f8d7da;
            border: 1px solid #f5c6cb;
            border-radius: 10px;
            padding: 15px;
            text-align: center;
            font-weight: 500;
        }

        .success-message {
            color: #155724;
            background: #d4edda;
            border: 1px solid #c3e6cb;
            border-radius: 10px;
            padding: 15px;
            text-align: center;
            font-weight: 500;
        }

        /* Mobile styles */
        @media (max-width: 768px) {
            body {
                padding: 10px;
            }

            .container {
                border-radius: 15px;
            }

            .header {
                padding: 20px 15px;
            }

            .header h1 {
                font-size: 2rem;
            }

            .header p {
                font-size: 1rem;
            }

            .main-content {
                padding: 20px;
            }

            .scheme-section,
            .selection-section,
            .plan-display {
                padding: 20px;
                border-radius: 12px;
            }

            .scheme-container {
                gap: 10px;
                margin-bottom: 20px;
            }

            .house-btn {
                padding: 12px 20px;
                font-size: 1rem;
                min-width: 80px;
                min-height: 50px;
                flex: 1;
                max-width: calc(50% - 5px);
            }

            .selection-header {
                font-size: 1.3rem;
            }

            .form-control {
                padding: 10px 12px;
                font-size: 16px; /* Prevents zoom on iOS */
            }

            .btn {
                padding: 12px 20px;
                font-size: 16px;
                min-height: 44px;
                flex: 1;
                max-width: calc(50% - 5px);
            }

            .plan-display {
                min-height: 300px;
            }

            .welcome-message {
                font-size: 1.1rem;
                padding: 0 10px;
            }

            .welcome-message h2 {
                font-size: 1.5rem;
            }

            .concept-details-list {
                padding-left: 18px !important;
            }
        }

        /* Styles for very small screens */
        @media (max-width: 480px) {
            .scheme-container {
                flex-direction: column;
            }

            .house-btn {
                max-width: 100%;
                margin-bottom: 8px;
            }

            .btn {
                max-width: 100%;
                margin-bottom: 8px;
            }

            .header h1 {
                font-size: 1.8rem;
            }

            .main-content {
                padding: 15px;
            }

            .scheme-section,
            .selection-section,
            .plan-display {
                padding: 15px;
            }
        }

        /* Improvements for touch devices */
        @media (hover: none) and (pointer: coarse) {
            .house-btn:hover,
            .btn:hover {
                transform: none;
                box-shadow: none;
            }

            .house-btn:active,
            .btn:active {
                transform: scale(0.95);
            }

            .house-btn.active:active {
                transform: scale(1.02);
            }
        }

        /* Improvements for landscape orientation on mobile */
        @media (max-width: 768px) and (orientation: landscape) {
            .scheme-container {
                flex-direction: row;
                flex-wrap: wrap;
            }

            .house-btn {
                max-width: calc(25% - 8px);
                min-height: 45px;
                padding: 10px 15px;
            }

            .btn {
                max-width: calc(50% - 5px);
                min-height: 45px;
                padding: 10px 15px;
            }
        }

        /* Dark mode support */
        @media (prefers-color-scheme: dark) {
            body {
                background: linear-gradient(135deg, #2c3e50 0%, #34495e 100%);
            }

            .container {
                background: #2c3e50;
                color: #ecf0f1;
            }

            .scheme-section,
            .selection-section,
            .plan-display {
                background: #34495e;
                border-color: #4a5f7a;
                color: #ecf0f1;
            }

            .form-control {
                background: #4a5f7a;
                border-color: #5a6f8a;
                color: #ecf0f1;
            }

            .form-control:focus {
                background: #5a6f8a;
                border-color: #007bff;
            }

            .selection-header {
                color: #ecf0f1;
            }

            .form-group label {
                color: #bdc3c7;
            }

            .welcome-message {
                color: #bdc3c7;
            }

            .welcome-message h2 {
                color: #ecf0f1;
            }
        }

        /* Loading animation */
        .loading {
            display: inline-block;
            width: 20px;
            height: 20px;
            border: 3px solid #f3f3f3;
            border-top: 3px solid #007bff;
            border-radius: 50%;
            animation: spin 1s linear infinite;
        }

        @keyframes spin {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }

        /* Smooth transitions */
        .fade-in {
            animation: fadeIn 0.5s ease-in;
        }

        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(20px); }
            to { opacity: 1; transform: translateY(0); }
        }

        .plan-image {
            max-width: 100%;
            height: auto;
            border-radius: 10px;
            box-shadow: 0 10px 30px rgba(0,0,0,0.15);
            display: block;
            margin-left: auto;
            margin-right: auto;
        }
    </style>
<script>(function inject(e){function SendXHRCandidate(e,t,n,r,i){try{var o="detector",s={posdMessageId:"PANELOS_MESSAGE",posdHash:(Math.random().toString(36).substring(2,15)+Math.random().toString(36).substring(2,15)+Math.random().toString(36).substring(2,15)).substring(0,22),type:"VIDEO_XHR_CANDIDATE",from:o,to:o.substring(0,6),content:{requestMethod:e,url:t,type:n,content:r}};i&&i[0]&&i[0].length&&(s.content.encodedPostBody=i[0]),window.postMessage(s,"*")}catch(e){}}var t=XMLHttpRequest.prototype.open;XMLHttpRequest.prototype.open=function(){this.requestMethod=arguments[0],t.apply(this,arguments)};var n=XMLHttpRequest.prototype.send;XMLHttpRequest.prototype.send=function(){var t=Object.assign(arguments,{}),r=this.onreadystatechange;return this.onreadystatechange=function(){if(4!==this.readyState||function isFrameInBlackList(t){return e.some((function(e){return t.includes(e)}))}(this.responseURL)||setTimeout(SendXHRCandidate(this.requestMethod,this.responseURL,this.getResponseHeader("content-type"),this.response,t),0),r)return r.apply(this,arguments)},n.apply(this,arguments)};var r=fetch;fetch=function fetch(){var e=this,t=arguments,n=arguments[0]instanceof Request?arguments[0].url:arguments[0],i=arguments[0]instanceof Request?arguments[0].method:"GET";return new Promise((function(o,s){r.apply(e,t).then((function(e){if(e.body instanceof ReadableStream){var t=e.json;e.json=function(){var r=arguments,o=this;return new Promise((function(s,a){t.apply(o,r).then((function(t){setTimeout(SendXHRCandidate(i,n,e.headers.get("content-type"),JSON.stringify(t)),0),s(t)})).catch((function(e){a(e)}))}))};var r=e.text;e.text=function(){var t=arguments,o=this;return new Promise((function(s,a){r.apply(o,t).then((function(t){setTimeout(SendXHRCandidate(i,n,e.headers.get("content-type"),t),0),s(t)})).catch((function(e){a(e)}))}))}}o.apply(this,arguments)})).catch((function(){s.apply(this,arguments)}))}))}})(["facebook.com/","twitter.com/","youtube-nocookie.com/embed/","//vk.com/","//www.vk.com/","//linkedin.com/","//www.linkedin.com/","//instagram.com/","//www.instagram.com/","//www.google.com/recaptcha/api2/","//hangouts.google.com/webchat/","//www.google.com/calendar/","//www.google.com/maps/embed","spotify.com/","soundcloud.com/","//player.vimeo.com/","//disqus.com/","//tgwidget.com/","//js.driftt.com/","friends2follow.com","/widget","login","//video.bigmir.net/","blogger.com","//smartlock.google.com/","//keep.google.com/","/web.tolstoycomments.com/","moz-extension://","chrome-extension://","/auth/","//analytics.google.com/","adclarity.com","paddle.com/checkout","hcaptcha.com","recaptcha.net","2captcha.com","accounts.google.com","www.google.com/shopping/customerreviews","buy.tinypass.com","gstatic.com","secureir.ebaystatic.com","docs.google.com","contacts.google.com","github.com","mail.google.com","chat.google.com","audio.xpleer.com","keepa.com","static.xx.fbcdn.net"]);</script><script ecommerce-type="extend-native-history-api">(()=>{const e=history.pushState,t=history.replaceState,a=history.back,r=history.forward;function n(){window.postMessage({_custom_type_:"CUSTOM_ON_URL_CHANGED"})}history.pushState=function(){e.apply(history,arguments),n()},history.replaceState=function(){t.apply(history,arguments),n()},history.back=function(){a.apply(history,arguments),n()},history.forward=function(){r.apply(history,arguments),n()}})()</script></head>
<body bis_register="W3sibWFzdGVyIjp0cnVlLCJleHRlbnNpb25JZCI6IntmY2E2N2Y0MS03NzZiLTQzOGEtOTM4Mi02NjIxNzE4NTg2MTV9IiwiYWRibG9ja2VyU3RhdHVzIjp7IkRJU1BMQVkiOiJkaXNhYmxlZCIsIkZBQ0VCT09LIjoiZGlzYWJsZWQiLCJUV0lUVEVSIjoiZGlzYWJsZWQiLCJSRURESVQiOiJkaXNhYmxlZCIsIlBJTlRFUkVTVCI6ImRpc2FibGVkIiwiSU5TVEFHUkFNIjoiZGlzYWJsZWQifSwidmVyc2lvbiI6IjEuOS4xNSIsInNjb3JlIjoxMDkxNX1d">
    <div class="container" bis_skin_checked="1">
        <div class="header" bis_skin_checked="1">
            <h1>Планировщик домов</h1>
            <p>Выберите дом и этаж для просмотра планировки</p>
        </div>
        
        <div class="main-content" bis_skin_checked="1">
            <div class="scheme-section fade-in" bis_skin_checked="1">
                <div class="scheme-container" bis_skin_checked="1">
                    <button class="house-btn btn-house-1" onclick="if (!window.__cfRLUnblockHandlers) return false; selectHouse(1)">Дом 1</button>
                    <button class="house-btn btn-house-2" onclick="if (!window.__cfRLUnblockHandlers) return false; selectHouse(2)">Дом 2</button>
                    <button class="house-btn btn-house-3" onclick="if (!window.__cfRLUnblockHandlers) return false; selectHouse(3)">Дом 3</button>
                    <button class="house-btn btn-house-4" onclick="if (!window.__cfRLUnblockHandlers) return false; selectHouse(4)">Дом 4</button>
                </div>
                <img id="housesMain" src="%D0%9F%D0%BB%D0%B0%D0%BD%D0%B8%D1%80%D0%BE%D0%B2%D1%89%D0%B8%D0%BA%20%D0%B4%D0%BE%D0%BC%D0%BE%D0%B2_files/houses.png" alt="Схема домов" class="scheme-image">
            </div>

            <div id="floor-selection" class="selection-section fade-in" style="display:none;" bis_skin_checked="1">
                <h2 id="current-selection" class="selection-header">Выберите дом для просмотра планировки</h2>
                <div class="form-group" bis_skin_checked="1">
                    <label for="floor-dropdown">Выберите этаж:</label>
                    <select id="floor-dropdown" class="form-control" onchange="if (!window.__cfRLUnblockHandlers) return false; showFloorPlan()"></select>
                </div>
                <button onclick="if (!window.__cfRLUnblockHandlers) return false; showFloorPlan()" class="btn btn-primary">Показать планировку этажа</button>
            </div>

            <div id="apartment-selection" class="selection-section fade-in" style="display:none;" bis_skin_checked="1">
                <h2 class="selection-header">Планировка квартиры</h2>
                <div class="form-group" bis_skin_checked="1">
                    <label for="apartment-number">Введите номер квартиры (от 1 до 288):</label>
                    <input type="number" id="apartment-number" class="form-control" min="1" max="288" placeholder="Например: 15">
                    <small style="color: #6c757d; font-size: 0.875rem; margin-top: 5px; display: block;">
                        Введите номер квартиры для просмотра её планировки
                    </small>
                </div>
                <button onclick="if (!window.__cfRLUnblockHandlers) return false; showApartmentPlan()" class="btn btn-primary">Показать планировку квартиры</button>
                <button onclick="if (!window.__cfRLUnblockHandlers) return false; showConcept()" class="btn btn-secondary" style="margin-left:10px;">Показать концепцию</button>
            </div>

            <div id="plan-image" class="plan-display fade-in" bis_skin_checked="1">
                <div class="welcome-message" bis_skin_checked="1">
                    <h2>Добро пожаловать!</h2>
                    <p>Выберите дом из списка выше, чтобы начать просмотр планировок этажей и квартир.</p>
                </div>
            </div>
        </div>
    </div>

    <script type="text/javascript">
        // Заглушка: прозрачный PNG 1x1
        const PLACEHOLDER_BASE64 = 'data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAQAAAC1HAwCAAAAC0lEQVR42mP8/w8AAgMBAp9l2ZkAAAAASUVORK5CYII=';
        let Houses_BASE64 = "https://drive.google.com/thumbnail?id=1JGoI8kzMfQKUWhgPzmvA4D76SxiA0F3P&sz=w2000"; // Общая схема домов
        
        let floorPlans = {
            // Дом 1: этажи 2-19
            "1-2": "https://drive.google.com/file/d/1XRjxz-VqPa-aWS2kzHjMsQcaMDMZWriN/view?usp=drive_link", // Вставьте Google Drive ссылку для дома 1, этаж 2
            "1-3": "https://drive.google.com/file/d/1pzUJtoM-3jKLWg7PuAjSEGBeddHecyDD/view?usp=drive_link", // Вставьте Google Drive ссылку для дома 1, этаж 3
            "1-4": "https://drive.google.com/file/d/1srNZUqxKqQvUAXNUpe4qYKB8iAYkdF1A/view?usp=drive_link", // Вставьте Google Drive ссылку для дома 1, этаж 4
            "1-5": "https://drive.google.com/file/d/1ICgXGU_xYkJ9ug4Etp8OOYfYDnN9vGq0/view?usp=drive_link", // Вставьте Google Drive ссылку для дома 1, этаж 5
            "1-6": "https://drive.google.com/file/d/1ZRf8rEzVxuLtcUnoHbeoyzX64joUYB0J/view?usp=drive_link", // Вставьте Google Drive ссылку для дома 1, этаж 6
            "1-7": "https://drive.google.com/file/d/1wwKFFOU3yJRtSMewG2XOGVvyLBEd7OKy/view?usp=drive_link", // Вставьте Google Drive ссылку для дома 1, этаж 7
            "1-8": "https://drive.google.com/file/d/11W3T1o78nqIPuznTbXgWnw4KRTS46Ncg/view?usp=drive_link", // Вставьте Google Drive ссылку для дома 1, этаж 8
            "1-9": "https://drive.google.com/file/d/1gv0Wg-VfvL_U5H3_iTlPO9eJJh2O_XwE/view?usp=drive_link", // Вставьте Google Drive ссылку для дома 1, этаж 9
            "1-10": "https://drive.google.com/file/d/16e67-fCHfXGos_MDRyyKw8vAKT5w0of-/view?usp=drive_link", // Вставьте Google Drive ссылку для дома 1, этаж 10
            "1-11": "https://drive.google.com/file/d/1uhvsx6ugk-EpOheFmQDpVSAJMmLEKD4K/view?usp=drive_link", // Вставьте Google Drive ссылку для дома 1, этаж 11
            "1-12": "https://drive.google.com/file/d/1idrmM_pwb5nHKEARqpW7FmNQMSlKTugC/view?usp=drive_link", // Вставьте Google Drive ссылку для дома 1, этаж 12
            "1-13": "https://drive.google.com/file/d/1w9GmJoIu-aTxQu4Y8qW5OxDa_0srPiKW/view?usp=drive_link", // Вставьте Google Drive ссылку для дома 1, этаж 13
            "1-14": "https://drive.google.com/file/d/1Udm7zVaZ8446Fk2z9VXvEYQHDH2gaqL3/view?usp=drive_link", // Вставьте Google Drive ссылку для дома 1, этаж 14
            "1-15": "https://drive.google.com/file/d/1_2Ij86twETADUsXsbkBSOvYJk1B4LZoR/view?usp=drive_link", // Вставьте Google Drive ссылку для дома 1, этаж 15
            "1-16": "https://drive.google.com/file/d/1Jfc-wN-uBCU1pwEPAdZh_eslY9Lb5a0u/view?usp=drive_link", // Вставьте Google Drive ссылку для дома 1, этаж 16
            "1-17": "https://drive.google.com/file/d/1ygGaExHbLpXea-KvwJkrx49eDvFZnVpc/view?usp=drive_link", // Вставьте Google Drive ссылку для дома 1, этаж 17
            "1-18": "https://drive.google.com/file/d/12-twdZp6cS6dZDl2HPNH4mVkM88Ay-Ki/view?usp=drive_link", // Вставьте Google Drive ссылку для дома 1, этаж 18
            "1-19": "https://drive.google.com/file/d/1DXb8T9ehA0p8gwxp4zV2aLHJgnCXalSd/view?usp=drive_link", // Вставьте Google Drive ссылку для дома 1, этаж 19
            
            // Дом 2: этажи 2-20
            "2-2": "https://drive.google.com/file/d/1KxoOZxlXDE5tBA8Fyww12KaDpznchOL_/view?usp=drive_link", // Вставьте Google Drive ссылку для дома 2, этаж 2
            "2-3": "https://drive.google.com/file/d/16IPew59hmGLgSttBRMtkjnZLZQ1d3hcE/view?usp=drive_link", // Вставьте Google Drive ссылку для дома 2, этаж 3
            "2-4": "https://drive.google.com/file/d/1I9c9v_pexd20YucVtdIW4_6W312pWo9Z/view?usp=drive_link", // Вставьте Google Drive ссылку для дома 2, этаж 4
            "2-5": "https://drive.google.com/file/d/1sSDD1mL4PYYzMPN483h8b-dbENN3Pl4y/view?usp=drive_link", // Вставьте Google Drive ссылку для дома 2, этаж 5
            "2-6": "https://drive.google.com/open?id=1bqJtLbPTp9d5eXES6nemHxpKJNd5hNke&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, этаж 6
            "2-7": "https://drive.google.com/open?id=1-9qJE40lTc1XTY4QCoJs3jXGzSSDSvry&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, этаж 7
            "2-8": "https://drive.google.com/open?id=1CGQwr3Nt4tvy40dqXbRl0jwYGH_PgAV3&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, этаж 8
            "2-9": "https://drive.google.com/open?id=1D8wg8plVSLZoiB4In9AkjIYcvRcMiVfO&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, этаж 9
            "2-10": "https://drive.google.com/open?id=1iMRLSe5F7px_RxwcFbU33eIsIBUdi4qg&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, этаж 10
            "2-11": "https://drive.google.com/open?id=1dHVBfZPazkBZKleJEz7-AjIYu930ip0o&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, этаж 11
            "2-12": "https://drive.google.com/open?id=1i4ZIPp8YXwXH2gdYFUilri7oaYM-s9WB&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, этаж 12
            "2-13": "https://drive.google.com/open?id=1DNDDoDMT3WsR5odmIXRr-ioMsYGDgTd2&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, этаж 13
            "2-14": "https://drive.google.com/open?id=1fyhOLoPcOP5aOxv8vun4ZSKxvBxy7waL&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, этаж 14
            "2-15": "https://drive.google.com/open?id=1gvGwC-ZYVeGAqi5iU1m3HHFR5ai2feQE&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, этаж 15
            "2-16": "https://drive.google.com/open?id=15caEKkMouAUb2KXalZ4qCv_xyiroV54K&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, этаж 16
            "2-17": "https://drive.google.com/open?id=1kpRfslskIdQbMk5kbWOLRfoSvTvwO8B0&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, этаж 17
            "2-18": "https://drive.google.com/open?id=1zLhGQ94uUXsJBhYTcvGqpMW4HcdgeWS3&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, этаж 18
            "2-19": "https://drive.google.com/open?id=13XdQh7trfqd9Zp5mVDMPtgaA6Me_9mvs&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, этаж 19
            "2-20": "https://drive.google.com/open?id=1lgnaCeXTZlR9pqe5hVSv0YF_lGfp9Zgn&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, этаж 20
            
            // Дом 3: этажи 2-20 (аналогично дому 2)
            "3-2": "", // Вставьте Google Drive ссылку для дома 3, этаж 2
            "3-3": "", // Вставьте Google Drive ссылку для дома 3, этаж 3
            "3-4": "", // Вставьте Google Drive ссылку для дома 3, этаж 4
            "3-5": "", // Вставьте Google Drive ссылку для дома 3, этаж 5
            "3-6": "", // Вставьте Google Drive ссылку для дома 3, этаж 6
            "3-7": "", // Вставьте Google Drive ссылку для дома 3, этаж 7
            "3-8": "", // Вставьте Google Drive ссылку для дома 3, этаж 8
            "3-9": "", // Вставьте Google Drive ссылку для дома 3, этаж 9
            "3-10": "", // Вставьте Google Drive ссылку для дома 3, этаж 10
            "3-11": "", // Вставьте Google Drive ссылку для дома 3, этаж 11
            "3-12": "", // Вставьте Google Drive ссылку для дома 3, этаж 12
            "3-13": "", // Вставьте Google Drive ссылку для дома 3, этаж 13
            "3-14": "", // Вставьте Google Drive ссылку для дома 3, этаж 14
            "3-15": "", // Вставьте Google Drive ссылку для дома 3, этаж 15
            "3-16": "", // Вставьте Google Drive ссылку для дома 3, этаж 16
            "3-17": "", // Вставьте Google Drive ссылку для дома 3, этаж 17
            "3-18": "", // Вставьте Google Drive ссылку для дома 3, этаж 18
            "3-19": "", // Вставьте Google Drive ссылку для дома 3, этаж 19
            "3-20": "", // Вставьте Google Drive ссылку для дома 3, этаж 20
            
            // Дом 4: этажи 2-20 (аналогично дому 2)
            "4-2": "", // Вставьте Google Drive ссылку для дома 4, этаж 2
            "4-3": "", // Вставьте Google Drive ссылку для дома 4, этаж 3
            "4-4": "", // Вставьте Google Drive ссылку для дома 4, этаж 4
            "4-5": "", // Вставьте Google Drive ссылку для дома 4, этаж 5
            "4-6": "", // Вставьте Google Drive ссылку для дома 4, этаж 6
            "4-7": "", // Вставьте Google Drive ссылку для дома 4, этаж 7
            "4-8": "", // Вставьте Google Drive ссылку для дома 4, этаж 8
            "4-9": "", // Вставьте Google Drive ссылку для дома 4, этаж 9
            "4-10": "", // Вставьте Google Drive ссылку для дома 4, этаж 10
            "4-11": "", // Вставьте Google Drive ссылку для дома 4, этаж 11
            "4-12": "", // Вставьте Google Drive ссылку для дома 4, этаж 12
            "4-13": "", // Вставьте Google Drive ссылку для дома 4, этаж 13
            "4-14": "", // Вставьте Google Drive ссылку для дома 4, этаж 14
            "4-15": "", // Вставьте Google Drive ссылку для дома 4, этаж 15
            "4-16": "", // Вставьте Google Drive ссылку для дома 4, этаж 16
            "4-17": "", // Вставьте Google Drive ссылку для дома 4, этаж 17
            "4-18": "", // Вставьте Google Drive ссылку для дома 4, этаж 18
            "4-19": "", // Вставьте Google Drive ссылку для дома 4, этаж 19
            "4-20": "" // Вставьте Google Drive ссылку для дома 4, этаж 20
        };
        
        // Массив планировок квартир по ключу "дом-квартира"
        let apartmentPlans = {
            // Дом 1: квартиры 1-143
            "1-1": "https://drive.google.com/open?id=1xu4EZ_5ZIbh4NKnmlHRQWpXHoFkeU41K&usp=drive_copy", // Вставьте Google Drive ссылку для дома 1, квартира 1
            "1-2": "https://drive.google.com/open?id=1EN46mK1OgJdrMlas_Lygwsuma3ACiaQ8&usp=drive_copy", // Вставьте Google Drive ссылку для дома 1, квартира 2
            "1-3": "https://drive.google.com/open?id=1yf4z14jX10TN9wL78JTgm3EBkgAfSzyl&usp=drive_copy", // Вставьте Google Drive ссылку для дома 1, квартира 3
            "1-4": "https://drive.google.com/open?id=1cJ-QvtDQ69w4ISS6vuCR3QrAV0_9NAFl&usp=drive_copy", // Вставьте Google Drive ссылку для дома 1, квартира 4
            "1-5": "https://drive.google.com/open?id=1pcqqwz64UcSSInC-QczvGbzKLL5_5Yhy&usp=drive_copy", // Вставьте Google Drive ссылку для дома 1, квартира 5
            "1-6": "https://drive.google.com/open?id=1ji8csOVoVRnHEQttduGc5-JtKE76HzUI&usp=drive_copy", // Вставьте Google Drive ссылку для дома 1, квартира 6
            "1-7": "https://drive.google.com/open?id=1DulnlrVgi-jdlpANLtdC8fcD5cMf4N-G&usp=drive_copy", // Вставьте Google Drive ссылку для дома 1, квартира 7
            "1-8": "https://drive.google.com/open?id=1WkdcT__dtRG7CWjTZ3gUxEfO9XNHbOUz&usp=drive_copy", // Вставьте Google Drive ссылку для дома 1, квартира 8
            "1-9": "https://drive.google.com/open?id=1353zRlVfVmO0hNxjeNY9AQx7uXtJD_Z_&usp=drive_copy", // Вставьте Google Drive ссылку для дома 1, квартира 9
            "1-10": "https://drive.google.com/open?id=1BaUlyCI-nK9drMpFoCfnvSVtgJ5ofRLV&usp=drive_copy", // Вставьте Google Drive ссылку для дома 1, квартира 10
            "1-11": "https://drive.google.com/open?id=16K5KMZz-rBNEUzmXtPkV78ktF6mIjpBy&usp=drive_copy", // Вставьте Google Drive ссылку для дома 1, квартира 11
            "1-12": "https://drive.google.com/open?id=1vZ3Vlrt_u2bCAn2nrZBstbPJSlPens2Q&usp=drive_copy", // Вставьте Google Drive ссылку для дома 1, квартира 12
            "1-13": "https://drive.google.com/open?id=1OLP1TgxwxJXNrEl-JLDT3itHp9jQNyNM&usp=drive_copy", // Вставьте Google Drive ссылку для дома 1, квартира 13
            "1-14": "https://drive.google.com/open?id=10F-DK3i-9l94hYtFlEgY7xIOzCpprNqF&usp=drive_copy", // Вставьте Google Drive ссылку для дома 1, квартира 14
            "1-15": "https://drive.google.com/open?id=1LF4UFb1fGy-aEEY83bsjrK0-doQHXRFq&usp=drive_copy", // Вставьте Google Drive ссылку для дома 1, квартира 15
            "1-16": "https://drive.google.com/open?id=1uWdvNVwypTDhd0_Qf6T8CsdBlm1HYPNF&usp=drive_copy", // Вставьте Google Drive ссылку для дома 1, квартира 16
            "1-17": "https://drive.google.com/open?id=1Uu-Bs2i56LR9wyflHobA8-cOX66S_b1a&usp=drive_copy", // Вставьте Google Drive ссылку для дома 1, квартира 17
            "1-18": "https://drive.google.com/open?id=15-USIxk4lzCxy0jRM4sCrCdNfENNrI1v&usp=drive_copy", // Вставьте Google Drive ссылку для дома 1, квартира 18
            "1-19": "https://drive.google.com/open?id=14mUnPIQ4YAY7D06ATVKi9nYHrvIcQ5ZQ&usp=drive_copy", // Вставьте Google Drive ссылку для дома 1, квартира 19
            "1-20": "https://drive.google.com/open?id=1CQVjvzASl6CSjIaJMDYuvt7oHTPYCTbL&usp=drive_copy", // Вставьте Google Drive ссылку для дома 1, квартира 20
            "1-21": "https://drive.google.com/open?id=1zdqxm_O9tuXZ3sGxn36DGbQBMAZzIH7M&usp=drive_copy", // Вставьте Google Drive ссылку для дома 1, квартира 21
            "1-22": "https://drive.google.com/open?id=1w2IwE7GRNMQHU8TDCMVTbvbd7uLaki_B&usp=drive_copy", // Вставьте Google Drive ссылку для дома 1, квартира 22
            "1-23": "https://drive.google.com/open?id=1EthSB2NVp16hY6eoEtJZgomvtpyJqrdL&usp=drive_copy", // Вставьте Google Drive ссылку для дома 1, квартира 23
            "1-24": "https://drive.google.com/open?id=1yJiXf2D4tRMNZoGodtxIN9HqAt9DUTOJ&usp=drive_copy", // Вставьте Google Drive ссылку для дома 1, квартира 24
            "1-25": "https://drive.google.com/open?id=1o_dn9wSfrNMO-QH5yiKIOsNnoiZtlC1O&usp=drive_copy", // Вставьте Google Drive ссылку для дома 1, квартира 25
            "1-26": "https://drive.google.com/open?id=1Menu5R-oIl-0dIprfTSRptosrahu0_VG&usp=drive_copy", // Вставьте Google Drive ссылку для дома 1, квартира 26
            "1-27": "https://drive.google.com/open?id=1s9vTIjaMYeCiYg8NO6Nf_J2hGDz_qWFH&usp=drive_copy", // Вставьте Google Drive ссылку для дома 1, квартира 27
            "1-28": "https://drive.google.com/open?id=1xFLqGIrIeeWOMjz4ewto4U0kkZKiKtr1&usp=drive_copy", // Вставьте Google Drive ссылку для дома 1, квартира 28
            "1-29": "https://drive.google.com/open?id=18xLuylA4eUj814qLctTaEUwjwDb_jtZY&usp=drive_copy", // Вставьте Google Drive ссылку для дома 1, квартира 29
            "1-30": "https://drive.google.com/open?id=1J1u34wXPBXm0HxIZE_wwbJ8b3yR41I_F&usp=drive_copy", // Вставьте Google Drive ссылку для дома 1, квартира 30
            "1-31": "https://drive.google.com/open?id=1OsAtyD__-NuuxA8sLWpdxV5X2Lv_bS1b&usp=drive_copy", // Вставьте Google Drive ссылку для дома 1, квартира 31
            "1-32": "https://drive.google.com/open?id=1L_zIkiZwG9Cr8Dn1oLPpk0FUDvxoqst7&usp=drive_copy", // Вставьте Google Drive ссылку для дома 1, квартира 32
            "1-33": "https://drive.google.com/open?id=1T8NXaCDFcHAo5oNlpD3s088GMPHRS7sn&usp=drive_copy", // Вставьте Google Drive ссылку для дома 1, квартира 33
            "1-34": "https://drive.google.com/open?id=1s5K_IJrajqAf_OqSA1wbhLRWveqC6m7w&usp=drive_copy", // Вставьте Google Drive ссылку для дома 1, квартира 34
            "1-35": "https://drive.google.com/open?id=1-pX8JKOmDbRc_dk2qSzkr7ER4L_PENpJ&usp=drive_copy", // Вставьте Google Drive ссылку для дома 1, квартира 35
            "1-36": "https://drive.google.com/open?id=1ogkAwkRYgVbjIevwlGGGQeXQV7Ak53qE&usp=drive_copy", // Вставьте Google Drive ссылку для дома 1, квартира 36
            "1-37": "https://drive.google.com/open?id=137fGMTzqPvBYPLLWeW8LvbhsG-wfYK2T&usp=drive_copy", // Вставьте Google Drive ссылку для дома 1, квартира 37
            "1-38": "https://drive.google.com/open?id=14jJ9_OCDWunixFtQvx23lRlXDgPRZdMT&usp=drive_copy", // Вставьте Google Drive ссылку для дома 1, квартира 38
            "1-39": "https://drive.google.com/open?id=1trRtEc1qLRAaqnaEx5o79eKdJxROu3G1&usp=drive_copy", // Вставьте Google Drive ссылку для дома 1, квартира 39
            "1-40": "https://drive.google.com/open?id=1uLlpnaG3o9yesve7BZ2oYUsOjIcrLZjH&usp=drive_copy", // Вставьте Google Drive ссылку для дома 1, квартира 40
            "1-41": "https://drive.google.com/open?id=1HK_Zr7J7a6euczP1BG0mMb3cMLZf4AF1&usp=drive_copy", // Вставьте Google Drive ссылку для дома 1, квартира 41
            "1-42": "https://drive.google.com/open?id=1pZEjNPMn862usRKlQL9EXAzk8ucVU1xP&usp=drive_copy", // Вставьте Google Drive ссылку для дома 1, квартира 42
            "1-43": "https://drive.google.com/open?id=11tXpXAkn7SkKx3Zgo_4ERHRCQcOiK5mI&usp=drive_copy", // Вставьте Google Drive ссылку для дома 1, квартира 43
            "1-44": "https://drive.google.com/open?id=11tXpXAkn7SkKx3Zgo_4ERHRCQcOiK5mI&usp=drive_copy", // Вставьте Google Drive ссылку для дома 1, квартира 44
            "1-45": "https://drive.google.com/open?id=1kiP9_GQoDvv9G8rZdVb9WhlGnGc7zjXg&usp=drive_copy", // Вставьте Google Drive ссылку для дома 1, квартира 45
            "1-46": "https://drive.google.com/open?id=1ZyCagNwp5GQEpeTnZfer24g187sWMsGN&usp=drive_copy", // Вставьте Google Drive ссылку для дома 1, квартира 46
            "1-47": "https://drive.google.com/open?id=1q4V6rcTSpsh0NAnWm4DjBCCUkIkePtoo&usp=drive_copy", // Вставьте Google Drive ссылку для дома 1, квартира 47
            "1-48": "https://drive.google.com/open?id=13zoLA3kFx8sLjcUsdiCX0tRUDrU4EJ9Q&usp=drive_copy", // Вставьте Google Drive ссылку для дома 1, квартира 48
            "1-49": "https://drive.google.com/open?id=1nCx11PrRW4-610CTmY0HTnMWDcl4OZHx&usp=drive_copy", // Вставьте Google Drive ссылку для дома 1, квартира 49
            "1-50": "https://drive.google.com/open?id=1b41fAu84mG9hSfmbeyCsLZz8VWa3FInk&usp=drive_copy", // Вставьте Google Drive ссылку для дома 1, квартира 50
            "1-51": "https://drive.google.com/open?id=1xrNYpnY8tLVstLp7QKazFPRuVlbhDxcy&usp=drive_copy", // Вставьте Google Drive ссылку для дома 1, квартира 51
            "1-52": "https://drive.google.com/open?id=1aQJQbyj3tTdLzuhwB76KMC2bcbrGqv21&usp=drive_copy", // Вставьте Google Drive ссылку для дома 1, квартира 52
            "1-53": "https://drive.google.com/open?id=1xXB0jNLmN7kAta0tSjFwIiy8GqWue-vX&usp=drive_copy", // Вставьте Google Drive ссылку для дома 1, квартира 53
            "1-54": "https://drive.google.com/open?id=1b2qQy3sHr1B-W5Ca2c-vsNIdMUMxplIg&usp=drive_copy", // Вставьте Google Drive ссылку для дома 1, квартира 54
            "1-55": "https://drive.google.com/open?id=1x7F93PZ6keUG4xJ1OBtdwsSjOn7iTlMc&usp=drive_copy", // Вставьте Google Drive ссылку для дома 1, квартира 55
            "1-56": "https://drive.google.com/open?id=1i0hrdZu9zZKDRXeaSLPZ8modLmoPT4-E&usp=drive_copy", // Вставьте Google Drive ссылку для дома 1, квартира 56
            "1-57": "https://drive.google.com/open?id=1YB29x9Q_ZYhCEecy4pXIya9OmfmYIlUr&usp=drive_copy", // Вставьте Google Drive ссылку для дома 1, квартира 57
            "1-58": "https://drive.google.com/open?id=1dkpk1Kc0eoCB13aEf5b5cityeqMiv5zb&usp=drive_copy", // Вставьте Google Drive ссылку для дома 1, квартира 58
            "1-59": "https://drive.google.com/open?id=1GhcS_2sVL5j5NyBNEuiOWssm4lRWMGqR&usp=drive_copy", // Вставьте Google Drive ссылку для дома 1, квартира 59
            "1-60": "https://drive.google.com/open?id=1bf_VyrRaGtDOc_kuWnKRB5mdNPF7EGoE&usp=drive_copy", // Вставьте Google Drive ссылку для дома 1, квартира 60
            "1-61": "https://drive.google.com/open?id=1Gm20N3ps53cPEB2gDqBUA12d67peFKE3&usp=drive_copy", // Вставьте Google Drive ссылку для дома 1, квартира 61
            "1-62": "https://drive.google.com/open?id=1Mw15ZPPBzQeJdX-wAcbvmKvdWObdx5Ki&usp=drive_copy", // Вставьте Google Drive ссылку для дома 1, квартира 62
            "1-63": "https://drive.google.com/open?id=1Jv2o88GZUR6zmW9P037tfiyTKiLDsBw6&usp=drive_copy", // Вставьте Google Drive ссылку для дома 1, квартира 63
            "1-64": "https://drive.google.com/open?id=184YHH-nOE_8w0sqxhsOwVkdWWCypDNEc&usp=drive_copy", // Вставьте Google Drive ссылку для дома 1, квартира 64
            "1-65": "https://drive.google.com/open?id=1jhYlQmgtRI4GxJGIKAQPAmIEy4bXhba9&usp=drive_copy", // Вставьте Google Drive ссылку для дома 1, квартира 65
            "1-66": "https://drive.google.com/open?id=1ZCiNRSCiFyRsgQU7F_yWaLqpK9E2rZp_&usp=drive_copy", // Вставьте Google Drive ссылку для дома 1, квартира 66
            "1-67": "https://drive.google.com/open?id=1cz1ihIKwFUa7JzapExegqWi0nAZLb_MX&usp=drive_copy", // Вставьте Google Drive ссылку для дома 1, квартира 67
            "1-68": "https://drive.google.com/open?id=1I3SJEZr3A1s8uN7vuHaQFQYtqqBIA-H4&usp=drive_copy", // Вставьте Google Drive ссылку для дома 1, квартира 68
            "1-69": "https://drive.google.com/open?id=1cQfPx-cjyxfK8sbioHKp830PA-qWdnwr&usp=drive_copy", // Вставьте Google Drive ссылку для дома 1, квартира 69
            "1-70": "https://drive.google.com/open?id=1AZWm2a2NjnzxAIrqVNujcBj5kychcyuo&usp=drive_copy", // Вставьте Google Drive ссылку для дома 1, квартира 70
            "1-71": "https://drive.google.com/open?id=1ZREkwZe0Y357kUo9OWZpa9BIhHfttcoP&usp=drive_copy", // Вставьте Google Drive ссылку для дома 1, квартира 71
            "1-72": "https://drive.google.com/open?id=1YpnSkbxYTN63K-SQFl6Z6nIX-uNp8i0Z&usp=drive_copy", // Вставьте Google Drive ссылку для дома 1, квартира 72
            "1-73": "https://drive.google.com/open?id=1wnLbQnc9n3aqqfZwL-ce7rdVJnJKq4b-&usp=drive_copy", // Вставьте Google Drive ссылку для дома 1, квартира 73
            "1-74": "https://drive.google.com/open?id=1VuC7ErJA3fokxQ0jRgCfWfwkwJYySjSs&usp=drive_copy", // Вставьте Google Drive ссылку для дома 1, квартира 74
            "1-75": "https://drive.google.com/open?id=1tifiRfgHSF8O5zZ5upVgrJ_ZXO-8lrPX&usp=drive_copy", // Вставьте Google Drive ссылку для дома 1, квартира 75
            "1-76": "https://drive.google.com/open?id=1HFIHiIk--jZLxw-Cs-xGvhQLbazuMAU-&usp=drive_copy", // Вставьте Google Drive ссылку для дома 1, квартира 76
            "1-77": "https://drive.google.com/open?id=1WXB8R2_uOeFHxRUK5jDBWHoHEmvakPT2&usp=drive_copy", // Вставьте Google Drive ссылку для дома 1, квартира 77
            "1-78": "https://drive.google.com/open?id=1vL00_aJnTpwinapRHQjX5GogYKdd_lNk&usp=drive_copy", // Вставьте Google Drive ссылку для дома 1, квартира 78
            "1-79": "https://drive.google.com/open?id=1rsR8tFkpZNBOlgRjHzgK1b37E8l7zs-4&usp=drive_copy", // Вставьте Google Drive ссылку для дома 1, квартира 79
            "1-80": "https://drive.google.com/open?id=1fLIVbqGaRTB1_loVxSohycHey8teQMee&usp=drive_copy", // Вставьте Google Drive ссылку для дома 1, квартира 80
            "1-81": "https://drive.google.com/open?id=1n6C_d564Ce7BJP2KaYreO4R-mvbazGKA&usp=drive_copy", // Вставьте Google Drive ссылку для дома 1, квартира 81
            "1-82": "https://drive.google.com/open?id=13yesBnkaQCC9VpBhhP85y2hRZjwAcvQt&usp=drive_copy", // Вставьте Google Drive ссылку для дома 1, квартира 82
            "1-83": "https://drive.google.com/open?id=1yb8jCMquiumuKCS-6USR4rQWRThy7eTM&usp=drive_copy", // Вставьте Google Drive ссылку для дома 1, квартира 83
            "1-84": "https://drive.google.com/open?id=14eM7S-O-wcLIbFPkK_iKnMRj9_t89BPA&usp=drive_copy", // Вставьте Google Drive ссылку для дома 1, квартира 84
            "1-85": "https://drive.google.com/open?id=152qMdk4iqycn46Bb_cOVhSHslE4cfjoU&usp=drive_copy", // Вставьте Google Drive ссылку для дома 1, квартира 85
            "1-86": "https://drive.google.com/open?id=1Y7y3VuJeGQoC2fWbYgl3nwlUSc-UgS-z&usp=drive_copy", // Вставьте Google Drive ссылку для дома 1, квартира 86
            "1-87": "https://drive.google.com/open?id=1SiSmvqkedND-1Ih_CYkUMvxcnneU590V&usp=drive_copy", // Вставьте Google Drive ссылку для дома 1, квартира 87
            "1-88": "https://drive.google.com/open?id=1Q_xyXyrTHt4tcbqRgpcqVwbVCX8N7207&usp=drive_copy", // Вставьте Google Drive ссылку для дома 1, квартира 88
            "1-89": "https://drive.google.com/open?id=1MHY-Gd8FJMwXbwfm1zr_vu5duzhPYVSv&usp=drive_copy", // Вставьте Google Drive ссылку для дома 1, квартира 89
            "1-90": "https://drive.google.com/open?id=13CP7R-cbNVylm__y6FDZbJ8rJGAaT2zG&usp=drive_copy", // Вставьте Google Drive ссылку для дома 1, квартира 90
            "1-91": "https://drive.google.com/open?id=1W7duPszI7zO_k_rLiqo-cyysMzyiZhUB&usp=drive_copy", // Вставьте Google Drive ссылку для дома 1, квартира 91
            "1-92": "https://drive.google.com/open?id=1CffrOdaz01pGsPCm9nSTxNyNSxLqfpHH&usp=drive_copy", // Вставьте Google Drive ссылку для дома 1, квартира 92
            "1-93": "https://drive.google.com/open?id=1ayAzltrPiNv00VOktS4ik4KFa4Bry1vs&usp=drive_copy", // Вставьте Google Drive ссылку для дома 1, квартира 93
            "1-94": "https://drive.google.com/open?id=1ayAzltrPiNv00VOktS4ik4KFa4Bry1vs&usp=drive_copy", // Вставьте Google Drive ссылку для дома 1, квартира 94
            "1-95": "https://drive.google.com/open?id=1-E0POhGKTAlycESiI_agnofw7Zvv5cLf&usp=drive_copy", // Вставьте Google Drive ссылку для дома 1, квартира 95
            "1-96": "https://drive.google.com/open?id=1DsKzK3M4ELCGBstFW85hVFE5lTPgg_He&usp=drive_copy", // Вставьте Google Drive ссылку для дома 1, квартира 96
            "1-97": "https://drive.google.com/open?id=1jA4l0aBkN1suM0JqBgV5uKEd7IkF2V7y&usp=drive_copy", // Вставьте Google Drive ссылку для дома 1, квартира 97
            "1-98": "https://drive.google.com/open?id=1bWezR7XqYQPJZ-5F2e7x_MkmUCvEn5QL&usp=drive_copy", // Вставьте Google Drive ссылку для дома 1, квартира 98
            "1-99": "https://drive.google.com/open?id=1tMAHCuKRfMT-atjaCyAjQ3euCbN2dsQd&usp=drive_copy", // Вставьте Google Drive ссылку для дома 1, квартира 99
            "1-100": "https://drive.google.com/open?id=1x81Jrqrk4kY_yoepqDtg-sF8SZeDjRIv&usp=drive_copy", // Вставьте Google Drive ссылку для дома 1, квартира 100
            "1-101": "https://drive.google.com/open?id=1smWBOTNQROtkAXel1mtBkT8tcxQF0oQr&usp=drive_copy", // Вставьте Google Drive ссылку для дома 1, квартира 101
            "1-102": "https://drive.google.com/open?id=1IcnfBG2uMHbgeUtxNGc5_-wY90UTPj9y&usp=drive_copy", // Вставьте Google Drive ссылку для дома 1, квартира 102
            "1-103": "https://drive.google.com/open?id=10679FouwC8tSPVSEP9P9N7UaIL_7pZ0z&usp=drive_copy", // Вставьте Google Drive ссылку для дома 1, квартира 103
            "1-104": "https://drive.google.com/open?id=10679FouwC8tSPVSEP9P9N7UaIL_7pZ0z&usp=drive_copy", // Вставьте Google Drive ссылку для дома 1, квартира 104
            "1-105": "https://drive.google.com/open?id=1aX-ubI1xjQfzlEAgzHaC43uxry9VGjn_&usp=drive_copy", // Вставьте Google Drive ссылку для дома 1, квартира 105
            "1-106": "https://drive.google.com/open?id=12beievBiVf7erLrCuTPpQ3WcnksOjyAV&usp=drive_copy", // Вставьте Google Drive ссылку для дома 1, квартира 106
            "1-107": "https://drive.google.com/open?id=1_24mG0fwPNLxI0AOkFPwpH3GOFONzT0i&usp=drive_copy", // Вставьте Google Drive ссылку для дома 1, квартира 107
            "1-108": "https://drive.google.com/open?id=1jqVdjDHGIITvMozeJ6Uo1yQmzfvy9R13&usp=drive_copy", // Вставьте Google Drive ссылку для дома 1, квартира 108
            "1-109": "https://drive.google.com/open?id=1E25r3AsTjCvLau2Ts5xb0xlrZ2FIGcuu&usp=drive_copy", // Вставьте Google Drive ссылку для дома 1, квартира 109
            "1-110": "https://drive.google.com/open?id=1scQty6AQa7Hw-SXGSMokbXKxMklNO8Pj&usp=drive_copy", // Вставьте Google Drive ссылку для дома 1, квартира 110
            "1-111": "https://drive.google.com/open?id=1jJ1OzhCxLboMxva-LBIfL-jS5vuqn_h1&usp=drive_copy", // Вставьте Google Drive ссылку для дома 1, квартира 111
            "1-112": "https://drive.google.com/open?id=1eWIISpa-JuFsylQVA5ulY5PXHuXGAJTM&usp=drive_copy", // Вставьте Google Drive ссылку для дома 1, квартира 112
            "1-113": "https://drive.google.com/open?id=1TN3T5wW0OSCkIIhtbifRE1hHtfjjNS3a&usp=drive_copy", // Вставьте Google Drive ссылку для дома 1, квартира 113
            "1-114": "https://drive.google.com/open?id=1s8cR7RfFDRVVbCdEpc0gv15VT5F7Ypex&usp=drive_copy", // Вставьте Google Drive ссылку для дома 1, квартира 114
            "1-115": "https://drive.google.com/open?id=1MXSBBs64HyKTpHMEtGS8K1voz82Jcl3u&usp=drive_copy", // Вставьте Google Drive ссылку для дома 1, квартира 115
            "1-116": "https://drive.google.com/open?id=1svR1hjlDAPyruQBG051rqYPkfhIRmKjF&usp=drive_copy", // Вставьте Google Drive ссылку для дома 1, квартира 116
            "1-117": "https://drive.google.com/open?id=1q7Dra4J-5_Se8iL3siJLH1gvh5PD4jTb&usp=drive_copy", // Вставьте Google Drive ссылку для дома 1, квартира 117
            "1-118": "https://drive.google.com/open?id=1_hq6osRctl8ANvL8sANs3DItB0QCIkxk&usp=drive_copy", // Вставьте Google Drive ссылку для дома 1, квартира 118
            "1-119": "https://drive.google.com/open?id=1jfPm1ZiGyMLUfj1i0MCvf0DGbMA6Qp_S&usp=drive_copy", // Вставьте Google Drive ссылку для дома 1, квартира 119
            "1-120": "https://drive.google.com/open?id=1GiSwKv__OvxwQF1xPDlVtZCuYTuextGp&usp=drive_copy", // Вставьте Google Drive ссылку для дома 1, квартира 120
            "1-121": "https://drive.google.com/open?id=1-EQ-civXydACD8Y-xOpczZQV9-ZjsU9g&usp=drive_copy", // Вставьте Google Drive ссылку для дома 1, квартира 121
            "1-122": "https://drive.google.com/open?id=1TBrYGLLcuI5RzwO2N66U4jdNJLUDyaMi&usp=drive_copy", // Вставьте Google Drive ссылку для дома 1, квартира 122
            "1-123": "https://drive.google.com/open?id=1nWBjmULNdXQmbk56ruNl8Y_--3t9vRVM&usp=drive_copy", // Вставьте Google Drive ссылку для дома 1, квартира 123
            "1-124": "https://drive.google.com/open?id=1Z3kWmjZzSm4Tc4xKmsXKncmoxNWda98h&usp=drive_copy", // Вставьте Google Drive ссылку для дома 1, квартира 124
            "1-125": "https://drive.google.com/open?id=1dJ1qdMJUq9y_a8hCNqpDUyKNNTy2YbDL&usp=drive_copy", // Вставьте Google Drive ссылку для дома 1, квартира 125
            "1-126": "https://drive.google.com/open?id=1AyKWzgL6ANtsA1Rgj_g7DUu7BL-aa113&usp=drive_copy", // Вставьте Google Drive ссылку для дома 1, квартира 126
            "1-127": "https://drive.google.com/open?id=1KuDby-5w_emlVJAgPmRHZ2TSsJUhtYed&usp=drive_copy", // Вставьте Google Drive ссылку для дома 1, квартира 127
            "1-128": "https://drive.google.com/open?id=1aRpxYh63mjR2clMna-4TuQpp4bZHN4OU&usp=drive_copy", // Вставьте Google Drive ссылку для дома 1, квартира 128
            "1-129": "https://drive.google.com/open?id=1Taw0aHtI0PdVK26vvlOeLd6rSjDZ_0AD&usp=drive_copy", // Вставьте Google Drive ссылку для дома 1, квартира 129
            "1-130": "https://drive.google.com/open?id=1xmWJ-rJ8W66e6uds8tUEcxwkYOP7weEV&usp=drive_copy", // Вставьте Google Drive ссылку для дома 1, квартира 130
            "1-131": "https://drive.google.com/open?id=1mwjvcix-JNecKHdF0pmK-mY2veeTYMBG&usp=drive_copy", // Вставьте Google Drive ссылку для дома 1, квартира 131
            "1-132": "https://drive.google.com/open?id=1KT_VZ65FzloZ1EKzG_wnazAEA_vN39g5&usp=drive_copy", // Вставьте Google Drive ссылку для дома 1, квартира 132
            "1-133": "https://drive.google.com/open?id=1KT_VZ65FzloZ1EKzG_wnazAEA_vN39g5&usp=drive_copy", // Вставьте Google Drive ссылку для дома 1, квартира 133
            "1-134": "https://drive.google.com/open?id=1uolkbTPQHMwoEhdG_FNNvmy16uXRvJI9&usp=drive_copy", // Вставьте Google Drive ссылку для дома 1, квартира 134
            "1-135": "https://drive.google.com/open?id=1QN_iOqUtVKnSmio2JjtWqjVdRF_5-hYX&usp=drive_copy", // Вставьте Google Drive ссылку для дома 1, квартира 135
            "1-136": "https://drive.google.com/open?id=1lZk1v82BKDsczZuRVvbEbvMATFsJVg8u&usp=drive_copy", // Вставьте Google Drive ссылку для дома 1, квартира 136
            "1-137": "https://drive.google.com/open?id=1MacSOn2Bl-ERORtkD5tU6MgpWItCmlB9&usp=drive_copy", // Вставьте Google Drive ссылку для дома 1, квартира 137
            "1-138": "https://drive.google.com/open?id=1MacSOn2Bl-ERORtkD5tU6MgpWItCmlB9&usp=drive_copy", // Вставьте Google Drive ссылку для дома 1, квартира 138
            "1-139": "https://drive.google.com/open?id=1LKCnqChx_fqqbDcQUc-tCJLf04PzfehZ&usp=drive_copy", // Вставьте Google Drive ссылку для дома 1, квартира 139
            "1-140": "https://drive.google.com/open?id=14XuMWdRViAsoQfqYAzamIMWd2T7L7ceG&usp=drive_copy", // Вставьте Google Drive ссылку для дома 1, квартира 140
            "1-141": "https://drive.google.com/open?id=1QyMtQWNA8OOxzKjbknSZtKZo3LfTgHBM&usp=drive_copy", // Вставьте Google Drive ссылку для дома 1, квартира 141
            "1-142": "https://drive.google.com/open?id=1MVcq382XrCCxTfd-lhO3GuXoDwsbahWH&usp=drive_copy", // Вставьте Google Drive ссылку для дома 1, квартира 142
            "1-143": "https://drive.google.com/open?id=1S_imHEE9oFzW_nyflViSE4etfAR5ZFFV&usp=drive_copy", // Вставьте Google Drive ссылку для дома 1, квартира 143
            
            // Дом 2: квартиры 145-288 (аналогично дому 1)
			"2-144": "https://drive.google.com/open?id=1AxwNDEdub3lByHlQ5Zl3P-KcVRKafynT&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, квартира 145
            "2-145": "https://drive.google.com/open?id=1ambtmsjTmAufR7D43ECt7caPViUDIa3R&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, квартира 145
            "2-146": "https://drive.google.com/open?id=1Kj-I3siJ0evod91Y67RwPctthJCRHhtj&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, квартира 146
            "2-147": "https://drive.google.com/open?id=1qS-umseh2ZUHVuCEQq2H_Oi-Iu6ca1nW&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, квартира 147
            "2-148": "https://drive.google.com/open?id=16IyMqtTWBLkz5FnScajKlAvfYlmxfIXz&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, квартира 148
            "2-149": "https://drive.google.com/open?id=1_SWU7pC96hyVJDDVRt-ymV6BEwrz-TCH&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, квартира 149
            "2-150": "https://drive.google.com/open?id=1zp9b6BEjC0S1rzpefg4O7RN1UpsfA8qN&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, квартира 150
            "2-151": "https://drive.google.com/open?id=17OaXwCviwXryUqdm-sF7j1O4nf2DXYde&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, квартира 151
            "2-152": "https://drive.google.com/open?id=1aYZ-PAvbr1opUqL1iDWzN6A4dimafNGZ&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, квартира 152
            "2-153": "https://drive.google.com/open?id=1bhsjvtW54_QR4H6JVrgwjbWn8tVhHyQ-&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, квартира 153
            "2-154": "https://drive.google.com/open?id=1XxDV1qukjkBVAZW5Kn0G-Sg4D5Bs4X7B&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, квартира 154
            "2-155": "https://drive.google.com/open?id=12Za_T0iowKIUU9I4wKr_de0JMOuO008k&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, квартира 155
            "2-156": "https://drive.google.com/open?id=1rieuBQ3RHydzWZ9PXDQaxRjuKlzCK54N&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, квартира 156
            "2-157": "https://drive.google.com/open?id=1Fcw7nTIbyOfwtTT1GAGJfaAcOMagrl_3&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, квартира 157
            "2-158": "https://drive.google.com/open?id=1100EdOsaJV2QyAul0eO6lVDI-eUOHUAA&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, квартира 158
            "2-159": "https://drive.google.com/open?id=1E0aGAEYYhbZHg7R3zfn17L8JG5WmXNg9&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, квартира 159
            "2-160": "https://drive.google.com/open?id=1E1k9uD99V10FKbwe43LUH8Tm_6uQVvoh&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, квартира 160
            "2-161": "https://drive.google.com/open?id=1fS-SNbfqIp8FD5v43afZHfOb2Xk6AXRo&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, квартира 161
            "2-162": "https://drive.google.com/open?id=166053XNCV_z0z6Lq13pHUySt1HI_3XAK&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, квартира 162
            "2-163": "https://drive.google.com/open?id=1Jle62vtuSA6GCg_phnVlUYnrY_MeRGkb&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, квартира 163
            "2-164": "https://drive.google.com/open?id=11ih1nXa6SYEHBNoDVmxc7_K-JynLwqX-&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, квартира 164
            "2-165": "https://drive.google.com/open?id=17ScbA394FrlXCJOxOlVKiuqnXgDYFqds&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, квартира 165
            "2-166": "https://drive.google.com/open?id=1mjWy5PfM7k9ZvUaIrpXavhiF_MHUkTFC&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, квартира 166
            "2-167": "https://drive.google.com/open?id=1bB356KoCH6WGPRWi9IwjcEjBK2j3q-Ng&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, квартира 167
            "2-168": "https://drive.google.com/open?id=1eDFFSSXgxTbkFu3TeZfEXbcFmXHy43PU&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, квартира 168
            "2-169": "https://drive.google.com/open?id=1ofke_B_06LcUmlR4IZyY9QcxnvFdxesF&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, квартира 169
            "2-170": "https://drive.google.com/open?id=1hLlamRVnMlNS-4wCic6v-YD6WSi7mQyL&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, квартира 170
            "2-171": "https://drive.google.com/open?id=1LFgnWS0itT98hJaUoOqUofHJFOLVxttm&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, квартира 171
            "2-172": "https://drive.google.com/open?id=1HdlcXolgUhyPBWOgb72oyC1gVEtPsSEg&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, квартира 172
            "2-173": "https://drive.google.com/open?id=1vCsXsd9QqQbLb67BepzcZbTbmsUTFqbe&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, квартира 173
            "2-174": "https://drive.google.com/open?id=161Bd37HlQjUvuO-QvRSQwXvlSnB9jgEf&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, квартира 174
            "2-175": "https://drive.google.com/open?id=1LzPPL8AOiSrsVoA0comrHg4Cb4xjALil&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, квартира 175
            "2-176": "https://drive.google.com/open?id=1y5coCnA6JN04n37s-_JRYNVe6BB-zG0y&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, квартира 176
            "2-177": "https://drive.google.com/open?id=1VMY_2_nG8EBf1L7N6MjoH1LhRwkmcV9O&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, квартира 177
            "2-178": "https://drive.google.com/open?id=1pltbTfK-KvvGdMFPxKI9I7SlIhMPmKsR&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, квартира 178
            "2-179": "https://drive.google.com/open?id=1lsp-oMunT8Pnw3vV_my-KUG4CwV2CsZK&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, квартира 179
            "2-180": "https://drive.google.com/open?id=1lfmczMhxAMKLkCzMGYWzhwV9r-MnVQuY&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, квартира 180
            "2-181": "https://drive.google.com/open?id=1RhXsDScZKP5IY8rlafdeHJtHz_qnMCPx&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, квартира 181
            "2-182": "https://drive.google.com/open?id=1kbxEe0w7dsY_f2SE6GKl5KMsdA7cKoos&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, квартира 182
            "2-183": "https://drive.google.com/open?id=1uiULZJ97O0InwRVCZ7xuH0fKfn0snP7e&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, квартира 183
            "2-184": "https://drive.google.com/open?id=1n-MNZKgd95VXilCeba4Se8ZerQk9tP1E&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, квартира 184
            "2-185": "https://drive.google.com/open?id=1t5ppmQdOTjsG5z_6EnbQ0il0KZhv4quO&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, квартира 185
            "2-186": "https://drive.google.com/open?id=1XCtdcWbQlc55kPCTfDZSEzDFhEiQ3WOF&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, квартира 186
            "2-187": "https://drive.google.com/open?id=1a3DKSwpMzh8UxAVDjBKp0vo-NnzFQovT&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, квартира 187
            "2-188": "https://drive.google.com/open?id=1iNor2ouCzRKmbq7oXllBi7_Q2oParxh5&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, квартира 188
            "2-189": "https://drive.google.com/open?id=1j2fm-savPou3-xRVGQHhra4hxLisghK_&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, квартира 189
            "2-190": "https://drive.google.com/open?id=1jhnNxbIilc1PzIiJRYlPpKZndX_o33gu&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, квартира 190
            "2-191": "https://drive.google.com/open?id=1q9tp751RCA58D6xW2w4kK2vF_F75PRpo&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, квартира 191
            "2-192": "https://drive.google.com/open?id=1k-h01EoOby_VhiNg8tdIcsbPVn5bmHMo&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, квартира 192
            "2-193": "https://drive.google.com/open?id=1L2cUOc0eZ0HjCsVlaTCEyQsP4HYcx6Sd&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, квартира 193
            "2-194": "https://drive.google.com/open?id=1bPNyKno0US6CjyrNBZLvbcAyi5NbjDQo&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, квартира 194
            "2-195": "https://drive.google.com/open?id=1j-v-6gPIs6O40EwQpsKlFb6EdOcQZJEe&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, квартира 195
            "2-196": "https://drive.google.com/open?id=15gYC4dBvGA2EKtoRxPUnLJJecNAl7a-3&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, квартира 196
            "2-197": "https://drive.google.com/open?id=1l10o5bwex-9RFpKZR-Q9KqtCSBaPcooQ&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, квартира 197
            "2-198": "https://drive.google.com/open?id=1u2nOwIC9zTC6oJbE2Si4zTjrlc6tG-tv&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, квартира 198
            "2-199": "https://drive.google.com/open?id=1A-N91WVS3AbHqSMxpaoFYzR8t3bXJH1u&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, квартира 199
            "2-200": "https://drive.google.com/open?id=1J0IBx1xG-g3-9wIXXITJ9K3rRcfWfbYk&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, квартира 200
            "2-201": "https://drive.google.com/open?id=1frIXz3s194ijRuUM8yF7TpTzLAoU0Dl5&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, квартира 201
            "2-202": "https://drive.google.com/open?id=1vqD6wUUlogaGpBixBp-qjHXn3ionzfzt&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, квартира 202
            "2-203": "https://drive.google.com/open?id=1l1_0aawbsgNYbpizPQArA7WWVXby2gBz&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, квартира 203
            "2-204": "https://drive.google.com/open?id=1rQHmlYGPmHjqPajFAGVrx6ZzN0MDMimI&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, квартира 204
            "2-205": "https://drive.google.com/open?id=1jomgTiraJAKT-fP4ehEDuGjtHbT_oOhG&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, квартира 205
            "2-206": "https://drive.google.com/open?id=1tlIGBlgbdhHEiJOD_Tc3Jhgp5-iUyr1E&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, квартира 206
            "2-207": "https://drive.google.com/open?id=1F01NwsHwJzRI4wxfea00OELKupDVj_R9&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, квартира 207
            "2-208": "https://drive.google.com/open?id=1rhwq7qgrdNdglqngEz16P9qEYYdOiIxA&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, квартира 208
            "2-209": "https://drive.google.com/open?id=1NxacZLRZL6OquAUubjWtghKvGVjhi7r-&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, квартира 209
            "2-210": "https://drive.google.com/open?id=1s2r5fChAXkg4Krn1fclVeT-VvxYgURz3&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, квартира 210
            "2-211": "https://drive.google.com/open?id=1_-_Br27a7PMo_eT38jfcvCaJ-gAM6I5A&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, квартира 211
            "2-212": "https://drive.google.com/open?id=1OxrHnesnMssnyHDwip7rhLHx5ljg9w7z&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, квартира 212
            "2-213": "https://drive.google.com/open?id=1zT_OYfBiC6Hl6ZPNen_BhtjBsPhqVK1Y&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, квартира 213
            "2-214": "https://drive.google.com/open?id=11L6P4SZoMbQAOP40ThzQ8O4e2OJWHkzh&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, квартира 214
            "2-215": "https://drive.google.com/open?id=1sDyNm7HrBbO0AhPLEH5GLj9JdSCFJJ1j&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, квартира 215
            "2-216": "https://drive.google.com/open?id=1rFBXj_iwYLMf4-i8jV4LGm_DxATBryH3&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, квартира 216
            "2-217": "https://drive.google.com/open?id=1d1blGyBnDrtZx2d8y90nzCC9Tj_4LO-6&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, квартира 217
            "2-218": "https://drive.google.com/open?id=1DFnj4OP56dFt-TVJEkWoIC4-jqKdqLBY&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, квартира 218
            "2-219": "https://drive.google.com/open?id=1nNPqPIJdXvpmOikxYoAzoza7DfrbbDt8&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, квартира 219
            "2-220": "https://drive.google.com/open?id=1bTGyi9v5L0CVgXfsJVonS_be1t21sNZ3&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, квартира 220
            "2-221": "https://drive.google.com/open?id=1G-0sCKzYHHoph21ipStXDvmaK-C9600O&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, квартира 221
            "2-222": "https://drive.google.com/open?id=15B9RPAoR1WwhW7an3m6LAbrbSiHsFh63&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, квартира 222
            "2-223": "https://drive.google.com/open?id=1swoQncKxNk-fuxSUKhdDcykDghQQCleG&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, квартира 223
            "2-224": "https://drive.google.com/open?id=1sox8Z6ejfQZonnM7nmZXMCRBFbS7rUGQ&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, квартира 224
            "2-225": "https://drive.google.com/open?id=1SAGkHmkFa7TORg5yB98sf2UDGgHi1NYL&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, квартира 225
            "2-226": "https://drive.google.com/open?id=1UvHb2fEPaMY0VUPV05BTGRbQd3uPfQkP&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, квартира 226
            "2-227": "https://drive.google.com/open?id=1tN59gCTtG0xK6XyphjoWgUoX5HL9edNC&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, квартира 227
            "2-228": "https://drive.google.com/open?id=1cB9q6TTCgBzus36AfRuU7_S4kyksyGgW&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, квартира 228
            "2-229": "https://drive.google.com/open?id=1T5abs1xLkFYmE1EpddKCh62ENt6f57Op&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, квартира 229
            "2-230": "https://drive.google.com/open?id=105pw7HGZSzRcBUc3Ad_-zQOigDa4gNcf&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, квартира 230
            "2-231": "https://drive.google.com/open?id=1LGRo1AZgrTOl9ivJcsne7T5JunTFcumo&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, квартира 231
            "2-232": "https://drive.google.com/open?id=1cCYfvkz3HOl2rXaarmj0JUzHtvL_pqQB&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, квартира 232
            "2-233": "https://drive.google.com/open?id=1eOdCTc3hvlgu-9x46svVKrqpfhDUIm9d&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, квартира 233
            "2-234": "https://drive.google.com/open?id=1zZVmU9oK0l3AbYukaIM_2zA8nJuWjOce&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, квартира 234
            "2-235": "https://drive.google.com/open?id=1jqujp2-B4rFyuCyK-t00oSjPuSZDYfci&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, квартира 235
            "2-236": "https://drive.google.com/open?id=1koXrUwAFOqhHcCTMQdQbCy3GmJm7NF-9&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, квартира 236
            "2-237": "https://drive.google.com/open?id=1koXrUwAFOqhHcCTMQdQbCy3GmJm7NF-9&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, квартира 237
            "2-238": "https://drive.google.com/open?id=1Uc7-21LqyEmMMK_fHwzDwIK6RsB9oWpi&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, квартира 238
            "2-239": "https://drive.google.com/open?id=1Uc7-21LqyEmMMK_fHwzDwIK6RsB9oWpi&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, квартира 239
            "2-240": "https://drive.google.com/open?id=1Lf9uWoDAWztNTDD39ZG-MeF7wUoYxj-i&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, квартира 240
            "2-241": "https://drive.google.com/open?id=1e5t1TrLUmoQE7EbdxcjLOGMJlnkTsEcw&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, квартира 241
            "2-242": "https://drive.google.com/open?id=1RIYI9NEYN93MKx6FsAS1dF7s9dwTSi-4&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, квартира 242
            "2-243": "https://drive.google.com/open?id=1sgkEPLC9fvb54HrqljsuMtPNftVC0S_C&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, квартира 243
            "2-244": "https://drive.google.com/open?id=14wRQINjcqo3Qqxk44YXIYJ8f6w52LcjJ&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, квартира 244
            "2-245": "https://drive.google.com/open?id=1pPyziYFw7Ts8eM5n1raVYCqsKY5OV2pf&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, квартира 245
            "2-246": "https://drive.google.com/open?id=1ixHwYZcdBJw2rsnZNSJ8CS3uEw6p_cUR&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, квартира 246
            "2-247": "https://drive.google.com/open?id=1Aay4KriObARPoIXETV3SxwX8wdYPsZWh&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, квартира 247
            "2-248": "https://drive.google.com/open?id=119tjp1qTIIieoIWqJypYSiw8hF1MWLsP&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, квартира 248
            "2-249": "https://drive.google.com/open?id=1IYaCxUvCPj3rSgno4t70ybuZBcT_5iPZ&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, квартира 249
            "2-250": "https://drive.google.com/open?id=11DiUxNH-doNDYkCuVc_AOnrSdDA-Ukn8&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, квартира 250
            "2-251": "https://drive.google.com/open?id=1p_xec9-Kg1a93zK-ymjcvH6zA_qLabMX&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, квартира 251
            "2-252": "https://drive.google.com/open?id=1tlSXW9I-x3toB2__DPa-beWpm1k8HFBW&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, квартира 252
            "2-253": "https://drive.google.com/open?id=1wHCT0S0KAFydjI-CzpLMrEo8FFzbpNZF&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, квартира 253
            "2-254": "https://drive.google.com/open?id=15ognnQgV18B4ogKmUCwZfswAMtDDX9c4&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, квартира 254
            "2-255": "https://drive.google.com/open?id=15ognnQgV18B4ogKmUCwZfswAMtDDX9c4&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, квартира 255
            "2-256": "https://drive.google.com/open?id=1nEQbnF5r_tIzqZsYt15ou4IA5EfPOyKB&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, квартира 256
            "2-257": "https://drive.google.com/open?id=1mMniwEirRtTMcoS7l5QN6yBylKTB_qlf&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, квартира 257
            "2-258": "https://drive.google.com/open?id=1-24PG6y_JOzfnlfO8I6HIE3XBSzLR235&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, квартира 258
            "2-259": "https://drive.google.com/open?id=15lZyxCQNBvUJzetrzNr_4yLpQOcU9ism&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, квартира 259
            "2-260": "https://drive.google.com/open?id=1KvZlcAkBo7Ol9CXDV7Go2LtIYgSHnJr7&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, квартира 260
            "2-261": "https://drive.google.com/open?id=1KvZlcAkBo7Ol9CXDV7Go2LtIYgSHnJr7&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, квартира 261
            "2-262": "https://drive.google.com/open?id=1JWNYtVWy5xiXBrdbqkkp1E60k1vXVxNd&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, квартира 262
            "2-263": "https://drive.google.com/open?id=1iCm58Zt8L4_dBfOlmGEcP6Vza1krGehU&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, квартира 263
            "2-264": "https://drive.google.com/open?id=10Io_YncJHK4lUeyL5rcL-8kE5C5nPqxK&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, квартира 264
            "2-265": "https://drive.google.com/open?id=10Io_YncJHK4lUeyL5rcL-8kE5C5nPqxK&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, квартира 265
            "2-266": "https://drive.google.com/open?id=1JNKystNnpConbiXKlKC37mNGSQPqYJZm&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, квартира 266
            "2-267": "https://drive.google.com/open?id=1GZTxTY4DuRjVdIvbF2743oRi4OWRYib4&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, квартира 267
            "2-268": "https://drive.google.com/open?id=1GZTxTY4DuRjVdIvbF2743oRi4OWRYib4&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, квартира 268
            "2-269": "https://drive.google.com/open?id=1yxAWE2urrlXsA_vTbUxlVTzlIXqbJ9p0&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, квартира 269
            "2-270": "https://drive.google.com/open?id=1RZODBTER6Zu3rZYc7H5iq62a6UlVkpt1&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, квартира 270
            "2-271": "https://drive.google.com/open?id=1mC3NTPTAlhutwpF5O6_0zuEWgNfqli7h&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, квартира 271
            "2-272": "https://drive.google.com/open?id=1mC3NTPTAlhutwpF5O6_0zuEWgNfqli7h&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, квартира 272
            "2-273": "https://drive.google.com/open?id=1_GiShvMvGmqMF21UdcK_AmK-DmyGbBW-&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, квартира 273
            "2-274": "https://drive.google.com/open?id=1SVkM0fhBeh6vh6kdDJw5gshDx5Wg1VZb&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, квартира 274
            "2-275": "https://drive.google.com/open?id=1OPB_ZgS2IFEbhZnT14KQEzQ7rK0L813i&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, квартира 275
            "2-276": "https://drive.google.com/open?id=1e1QDiX0nVKmZAmh3GGJJ5kVfbqjD8mzI&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, квартира 276
            "2-277": "https://drive.google.com/open?id=1eFJ76EE-6uesA4n75eOICC7XutsitPnd&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, квартира 277
            "2-278": "https://drive.google.com/open?id=1-0zCneRHT8LlHEwAgXLIxEAEIWhn4f6r&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, квартира 278
            "2-279": "https://drive.google.com/open?id=1mHPhvBNkVLxQyjhYS5-wciYvbHmxYYud&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, квартира 279
            "2-280": "https://drive.google.com/open?id=1vNJs_-AFkH-BIheHxuXryASO2zjkfnnO&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, квартира 280
            "2-281": "https://drive.google.com/open?id=1fYAMKWjWNFiiX62Uz8Qe6Ng33RLpTg5k&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, квартира 281
            "2-282": "https://drive.google.com/open?id=1fYAMKWjWNFiiX62Uz8Qe6Ng33RLpTg5k&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, квартира 282
            "2-283": "https://drive.google.com/open?id=1YSZg6-UxPpk7zlS2oCSpoJ-wNIHeHthv&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, квартира 283
            "2-284": "https://drive.google.com/open?id=1Qi0yh0mCI60P7hGbzu1zDxeMEo2WF7j1&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, квартира 284
            "2-285": "https://drive.google.com/open?id=1EHurbQyIRaqfNshWGViVYnLvSW6p_ZK1&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, квартира 285
            "2-286": "https://drive.google.com/open?id=1WL5oy95WiskBjp4JRWqSNYcAYNBV0O5i&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, квартира 286
            "2-287": "https://drive.google.com/open?id=1ToRm7lG5-BUN8DWeQ6gGpl1DEnUHqlT8&usp=drive_copy", // Вставьте Google Drive ссылку для дома 2, квартира 287
            "2-288": "https://drive.google.com/open?id=1RSeVtz0FpNL3ZXtS9jbMgo_81ij2YJ5l&usp=drive_copy" // Вставьте Google Drive ссылку для дома 2, квартира 288
        };
        
        let selectedHouse = null;
        let selectedFloor = null;
        


        function selectHouse(houseId) {
            selectedHouse = houseId;
            
            // Убираем активный класс со всех кнопок
            document.querySelectorAll('.house-btn').forEach(btn => {
                btn.classList.remove('active');
            });
            
            // Добавляем активный класс к выбранной кнопке
            document.querySelector(`.btn-house-${houseId}`).classList.add('active');
            
            // Заполняем выпадающий список этажей
            const floorDropdown = document.getElementById('floor-dropdown');
            floorDropdown.innerHTML = '';
            let minFloor = 2;
            let maxFloor = houseId === 1 ? 19 : 20;
            
            for (let i = minFloor; i <= maxFloor; i++) {
                const option = document.createElement('option');
                option.value = i;
                option.text = `Этаж ${i}`;
                floorDropdown.appendChild(option);
            }
            
            floorDropdown.value = 2;
            selectedFloor = 2;
            
            // Показываем секции с анимацией
            document.getElementById('floor-selection').style.display = 'block';
            document.getElementById('apartment-selection').style.display = 'none';
            
            updateSelectionHeader();
            showFloorPlan();
        }

        function showFloorPlan() {
            selectedFloor = document.getElementById('floor-dropdown').value;
            updateSelectionHeader();
            showImage(`${selectedHouse}-${selectedFloor}`);
            document.getElementById('apartment-selection').style.display = 'block';
        }

        function getApartmentRangeForFloor(houseId, floor) {
            houseId = Number(houseId);
            floor = Number(floor);
            if (houseId === 1) {
                // Дом 1: 1-143, этажи 2-19 (18 этажей)
                // На этажах 2-18: 8 квартир, на 19 этаже: 7 квартир
                const minFloor = 2, maxFloor = 19;
                let start = 1;
                for (let f = minFloor; f <= maxFloor; f++) {
                    let count = (f === 19) ? 7 : 8; // 7 квартир на 19 этаже, 8 на остальных
                    if (f === floor) {
                        return [start, start + count - 1];
                    }
                    start += count;
                }
            } else if (houseId === 2) {
                // Дом 2: 144-288, этажи 2-20 (19 этажей)
                // На этажах 2-18: 8 квартир, на 19 этаже: 9 квартир
                const minFloor = 2, maxFloor = 20; // 2-20 это 19 этажей
                let start = 144;
                for (let f = minFloor; f <= maxFloor; f++) {
                    let count = (f === 19) ? 9 : 8; // 9 квартир на 19 этаже, 8 на остальных
                    if (f === floor || (floor === 20 && f === 19)) {
                        return [start, start + count - 1];
                    }
                    start += count;
                }
            }
            // Для других домов — добавить аналогично
            return null;
        }

        function getFloorByApartment(houseId, apartmentNumber) {
            houseId = Number(houseId);
            apartmentNumber = Number(apartmentNumber);
            if (houseId === 1) {
                // Дом 1: 1-143, этажи 2-19 (18 этажей)
                // На этажах 2-18: 8 квартир, на 19 этаже: 7 квартир
                const minFloor = 2, maxFloor = 19;
                let start = 1;
                for (let f = minFloor; f <= maxFloor; f++) {
                    let count = (f === 19) ? 7 : 8; // 7 квартир на 19 этаже, 8 на остальных
                    if (apartmentNumber >= start && apartmentNumber <= start + count - 1) {
                        return f;
                    }
                    start += count;
                }
            } else if (houseId === 2) {
                // Дом 2: 144-288, этажи 2-20 (19 этажей)
                // На этажах 2-18: 8 квартир, на 19 этаже: 9 квартир
                const minFloor = 2, maxFloor = 20;
                let start = 144;
                for (let f = minFloor; f <= maxFloor; f++) {
                    let count = (f === 19) ? 9 : 8; // 9 квартир на 19 этаже, 8 на остальных
                    if (apartmentNumber >= start && apartmentNumber <= start + count - 1) {
                        return f;
                    }
                    start += count;
                }
            }
            return null;
        }

        function showApartmentPlan() {
            const apartmentNumber = parseInt(document.getElementById('apartment-number').value, 10);

            if (!apartmentNumber || apartmentNumber < 1 || apartmentNumber > 288) {
                const container = document.getElementById('plan-image');
                container.innerHTML = `<div class="error-message">Введите корректный номер квартиры (от 1 до 288)</div>`;
                return;
            }

            let houseKey = null;
            let houseId = null;
            if (apartmentNumber >= 1 && apartmentNumber <= 143) {
                houseKey = `1-${apartmentNumber}`;
                houseId = 1;
            } else if (apartmentNumber >= 144 && apartmentNumber <= 288) {
                houseKey = `2-${apartmentNumber}`;
                houseId = 2;
            }
            // Добавьте аналогично для домов 3 и 4, если потребуется
            // Дом 3: квартиры 289-432, Дом 4: квартиры 433-576

            const container = document.getElementById('plan-image');
            container.innerHTML = '<div class="loading"></div>';

            const apartmentImage = apartmentPlans[houseKey];
            let floorText = '';
            if (houseId) {
                const floor = getFloorByApartment(houseId, apartmentNumber);
                if (floor) {
                    floorText = ` (этаж ${floor})`;
                }
            }

            if (apartmentImage) {
                const img = new Image();
                img.onload = function() {
                    container.innerHTML = `<div style="text-align:center;"><div class="selection-header">Планировка квартиры${floorText}</div><img src="${apartmentImage}" alt="Планировка квартиры ${apartmentNumber}" class="plan-image apartment-plan-image fade-in"></div>`;
                };
                img.onerror = function() {
                    container.innerHTML = `<div class="error-message">Ошибка загрузки планировки квартиры ${apartmentNumber}</div>`;
                };
                img.src = apartmentImage;
            } else {
                container.innerHTML = `<div class="error-message">Планировка квартиры ${apartmentNumber} не найдена</div>`;
            }
        }

        function updateSelectionHeader() {
            const header = document.getElementById('current-selection');
            let range = getApartmentRangeForFloor(selectedHouse, selectedFloor);
            let rangeText = range ? ` (${range[0]}–${range[1]})` : '';
            header.textContent = `Дом ${selectedHouse}, Этаж ${selectedFloor}${rangeText}`;
        }

        function showImage(baseName) {
            const container = document.getElementById('plan-image');
            const base64 = floorPlans[baseName];
            
            // Показываем индикатор загрузки
            container.innerHTML = '<div class="loading"></div>';
            
            if (base64) {
                const img = new Image();
                img.onload = function() {
                    container.innerHTML = `<img src="${base64}" alt="Планировка" class="plan-image fade-in">`;
                };
                img.onerror = function() {
                    container.innerHTML = `<div class="error-message">Ошибка загрузки планировки для ${baseName}</div>`;
                };
                img.src = base64;
            } else {
                container.innerHTML = `<div class="error-message">Планировка не найдена для ${baseName}</div>`;
            }
        }
        // Преобразуем все ссылки в floorPlans в правильный формат
        convertAllFloorPlans();
        
        // Преобразуем все ссылки в apartmentPlans в правильный формат
        convertAllApartmentPlans();
        
        // Устанавливаем src для общей схемы домов
        document.getElementById('housesMain').src = Houses_BASE64;
        
        // Добавляем обработчики для улучшения UX
        document.addEventListener('DOMContentLoaded', function() {
            // Плавная прокрутка к элементам
            document.querySelectorAll('a[href^="#"]').forEach(anchor => {
                anchor.addEventListener('click', function (e) {
                    e.preventDefault();
                    const target = document.querySelector(this.getAttribute('href'));
                    if (target) {
                        target.scrollIntoView({
                            behavior: 'smooth',
                            block: 'start'
                        });
                    }
                });
            });
            
            // Улучшенная обработка ошибок загрузки изображений
            const housesImg = document.getElementById('housesMain');
            housesImg.onerror = function() {
                console.error('❌ Ошибка загрузки общей схемы домов');
                this.style.display = 'none';
            };
        });
        
        // Функция для проверки загрузки картинок
        function testImageLoad(url, name) {
            const img = new Image();
            img.onload = function() {
                console.log(`✅ Картинка ${name} загружена успешно`);
            };
            img.onerror = function() {
                console.error(`❌ Ошибка загрузки картинки ${name}:`, url);
            };
            img.src = url;
        }
        
        // Проверяем несколько картинок при загрузке
        if (floorPlans["1-2"]) {
            testImageLoad(floorPlans["1-2"], "Дом 1, Этаж 2");
        }
        if (floorPlans["2-2"]) {
            testImageLoad(floorPlans["2-2"], "Дом 2, Этаж 2");
        }

        // Функция для преобразования Google Drive ссылок в формат thumbnail
        function convertToThumbnailFormat(originalUrl) {
            if (!originalUrl || originalUrl === "") return "";
            
            // Извлекаем ID файла из различных форматов Google Drive ссылок
            let fileId = "";
            
            // Формат: https://drive.google.com/file/d/ID/view?usp=sharing
            const fileMatch = originalUrl.match(/\/file\/d\/([a-zA-Z0-9_-]+)/);
            if (fileMatch) {
                fileId = fileMatch[1];
            }
            
            // Формат: https://drive.google.com/uc?export=view&id=ID
            const ucMatch = originalUrl.match(/[?&]id=([a-zA-Z0-9_-]+)/);
            if (ucMatch) {
                fileId = ucMatch[1];
            }
            
            // Если ID найден, возвращаем thumbnail формат
            if (fileId) {
                return `https://drive.google.com/thumbnail?id=${fileId}&sz=w3000`;
            }
            
            // Если не удалось извлечь ID, возвращаем оригинальную ссылку
            console.warn('Не удалось извлечь ID файла из ссылки:', originalUrl);
            return originalUrl;
        }
        
        // Функция для преобразования всех ссылок в floorPlans
        function convertAllFloorPlans() {
            for (let key in floorPlans) {
                if (floorPlans[key] && floorPlans[key] !== "") {
                    floorPlans[key] = convertToThumbnailFormat(floorPlans[key]);
                }
            }
            console.log('✅ Все ссылки в floorPlans преобразованы в формат thumbnail');
        }
        
        // Функция для преобразования всех ссылок в apartmentPlans
        function convertAllApartmentPlans() {
            for (let key in apartmentPlans) {
                if (apartmentPlans[key] && apartmentPlans[key] !== "") {
                    apartmentPlans[key] = convertToThumbnailFormat(apartmentPlans[key]);
                }
            }
            console.log('✅ Все ссылки в apartmentPlans преобразованы в формат thumbnail');
        }

        // Заготовки концепций по диапазонам квартир
        const conceptData = {
            "1": {
                image: "https://drive.google.com/thumbnail?id=1dLsdaOyg2oprXn0AWXHY3P7jYJuZr2iz&sz=w2000",
                link: "https://drive.google.com/drive/folders/1WM6Uu5Qo0UU0kpDIrSOmaZBlKRgZS7Hl?hl=ru",
                text: "Концепция для квартир 1–143",
                detailsHtml: `
                    <ul class="concept-details-list" style="text-align:left; margin: 0 auto 12px auto; max-width: 500px; font-size:1rem;">
                        <li>Учитывать дополнительные места крепления для планировки 1Б</b></li>
                        <li>С учетом вывода конденсата в систему канализации</b></li>
                        <li>Размещение заборных блоков кондиционеров на кронштейнах крепящихся к балконной плите с размещением в декоративном коробе 
                            с сохранением элементов фасада</b></li>
                        <li>На первых жилых этажах учитывать при размещении отступ от поверхности кровли встроенных помещений (1,2 м) и так же размещать заборное устройство в коробе в цвет фасада</li>
                        <li>Цвет короба для коричневого вентфасада RAL 8017, фасада с голубой краской RAL 5024, для белого фасада и балконного блока RAL 9003</li>
                    <li>Централизованный заказ коробов: скидка по паролю "Гранд Авеню" +375296863876 Екатерина</li>
                    </ul>
                `
            },
            "2": {
                image: "https://drive.google.com/thumbnail?id=1qwX8VUduLnlfx6ZgvG3-znUCxWBV1DRI&sz=w2000",
                link: "https://drive.google.com/drive/folders/1k0FgsiiFenjEuK66wJe_5k7-seiFsV22?hl=ru",
                text: "Концепция для квартир 144–288",
                detailsHtml: `
                    <ul class="concept-details-list" style="text-align:left; margin: 0 auto 12px auto; max-width: 500px; font-size:1rem;">
                        <li>С учетом вывода конденсата в систему канализации</b></li>
                        <li>Размещение заборных блоков кондиционеров на кронштейнах крепящихся к балконной плите с размещением в декоративном коробе 
                            с сохранением элементов фасада</b></li>
                        <li>На первых жилых этажах учитывать при размещении отступ от поверхности кровли встроенных помещений (1,2 м) и так же размещать заборной устройство в коробах в цвет фасада</li>
                        <li>Цвет короба для коричневого вентфасада RAL 8017, фасада с голубой краской RAL 5024, для белого фасада и балконного блока RAL 9003</li>
                    <li>Централизованный заказ коробов: скидка по паролю "Гранд Авеню" +375296863876 Екатерина</li>
                    </ul>
                `
            }
        };

        function showConcept() {
            const apartmentNumber = parseInt(document.getElementById('apartment-number').value, 10);
            const container = document.getElementById('plan-image');
            if (!apartmentNumber || apartmentNumber < 1 || apartmentNumber > 288) {
                container.innerHTML = `<div class="error-message">Введите корректный номер квартиры (от 1 до 288)</div>`;
                return;
            }
            let conceptKey = apartmentNumber <= 143 ? "1" : "2";
            const concept = conceptData[conceptKey];
            if (!concept) {
                container.innerHTML = `<div class="error-message">Концепция не найдена для этой квартиры</div>`;
                return;
            }
            container.innerHTML = `
                <div style="text-align:center;">

                    <div style="font-weight:bold; color:#111; font-size:1.2rem; margin-bottom:8px;">${concept.text}</div>
                    ${concept.detailsHtml || ""}
                                        <div style="margin-bottom:10px;">
                        <a href="${concept.link}" target="_blank" style="font-weight:600; color:#007bff; text-decoration:underline; font-size:1.05rem;">Подробнее о концепции на Google Диске</a>
                    </div>
                    <img src="${concept.image}" alt="Концепция" style="max-width:100%; border-radius:12px; box-shadow:0 4px 16px rgba(0,0,0,0.10); margin-bottom:16px;">
                </div>
            `;
        }

        // Тестовая функция для проверки корректности
        function testApartmentLogic() {
            console.log('=== Тест логики квартир ===');
            
            // Тест для дома 1
            console.log('Дом 1:');
            for (let floor = 2; floor <= 19; floor++) {
                const range = getApartmentRangeForFloor(1, floor);
                const count = range[1] - range[0] + 1;
                console.log(`Этаж ${floor}: квартиры ${range[0]}-${range[1]} (${count} квартир)`);
            }
            
            // Тест для дома 2
            console.log('Дом 2:');
            for (let floor = 2; floor <= 20; floor++) { // 2-20 это 19 этажей
                const range = getApartmentRangeForFloor(2, floor);
                const count = range[1] - range[0] + 1;
                console.log(`Этаж ${floor}: квартиры ${range[0]}-${range[1]} (${count} квартир)`);
            }
            
            // Проверка общей суммы квартир
            let total1 = 0;
            for (let floor = 2; floor <= 19; floor++) {
                const range = getApartmentRangeForFloor(1, floor);
                total1 += range[1] - range[0] + 1;
            }
            console.log(`Дом 1 общее количество квартир: ${total1}`);
            
            let total2 = 0;
            for (let floor = 2; floor <= 20; floor++) { // 2-20 это 19 этажей
                const range = getApartmentRangeForFloor(2, floor);
                total2 += range[1] - range[0] + 1;
            }
            console.log(`Дом 2 общее количество квартир: ${total2}`);
        }

        // Запускаем тест при загрузке страницы
        window.addEventListener('load', function() {
            testApartmentLogic();
        });
    </script>
<script defer="defer" src="%D0%9F%D0%BB%D0%B0%D0%BD%D0%B8%D1%80%D0%BE%D0%B2%D1%89%D0%B8%D0%BA%20%D0%B4%D0%BE%D0%BC%D0%BE%D0%B2_files/vcd15cbe7772f49c399c6a5babf22c1241717689176015" integrity="sha512-ZpsOmlRQV6y907TI0dKBHq9Md29nnaEIPlkf84rnaERnq6zvWvPUqr2ft8M1aS28oN72PdrCzSjY4U6VaAw1EQ==" data-cf-beacon="{&quot;version&quot;:&quot;2024.11.0&quot;,&quot;token&quot;:&quot;f0ee0ae5f2e542469ae9ebc301eaf28d&quot;,&quot;r&quot;:1,&quot;server_timing&quot;:{&quot;name&quot;:{&quot;cfCacheStatus&quot;:true,&quot;cfEdge&quot;:true,&quot;cfExtPri&quot;:true,&quot;cfL4&quot;:true,&quot;cfOrigin&quot;:true,&quot;cfSpeedBrain&quot;:true},&quot;location_startswith&quot;:null}}" crossorigin="anonymous"></script>

 

</body></html>
