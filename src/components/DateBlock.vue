<template>
    <div class="DateBlock" @click="download">
        <div v-if="block.data.type == 'countdown'" class="cw-date-countdown">
            <div class="cw-date-countdown-digit" data-countdown="days">
                <div class="cw-date-countdown-number">{{countdownDays}}</div>
                <div v-show="countdownDays == '01'" class="cw-date-countdown-label-sg">Tag</div>
                <div v-show="countdownDays != '01'" class="cw-date-countdown-label-pl">Tage</div>
            </div>
            <div class="cw-date-countdown-digit" data-countdown="hours">
                <div class="cw-date-countdown-number">{{countdownHours}}</div>
                <div v-show="countdownHours == '01'" class="cw-date-countdown-label-sg">Stunde</div>
                <div v-show="countdownHours != '01'" class="cw-date-countdown-label-pl">Stunden</div>
            </div>
            <div class="cw-date-countdown-digit" data-countdown="minutes">
                <div class="cw-date-countdown-number">{{countdownMinutes}}</div>
                <div v-show="countdownMinutes == '01'" class="cw-date-countdown-label-sg">Minute</div>
                <div v-show="countdownMinutes != '01'"  class="cw-date-countdown-label-pl">Minuten</div>
            </div>
            <div class="cw-date-countdown-digit" data-countdown="seconds">
                <div class="cw-date-countdown-number">{{countdownSeconds}}</div>
                <div v-show="countdownSeconds == '01'" class="cw-date-countdown-label-sg">Sekunde</div>
                <div v-show="countdownSeconds != '01'" class="cw-date-countdown-label-pl">Sekunden</div>
            </div>
        </div>
        <div v-if="block.data.type == 'date'" class="cw-date-date">
            <div class="cw-date-date-digits" data-date="date">
                <div class="cw-date-date-number">{{dateDate}}</div>
            </div>
            <div class="cw-date-date-space"></div>
            <div class="cw-date-date-digits" data-date="time">
                <div class="cw-date-date-number">{{dateTime}}</div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'DateBlock',
    props:{
        block: Object,
        coursewarePath: String
    },
    data() {
        return{
            countdownDays: '00',
            countdownHours: '00',
            countdownMinutes: '00',
            countdownSeconds: '00',
        }
    },
    computed: {
        date() {
            return new Date(this.block.data.date + " "+ this.block.data.time);
        },
        timestamp() {
            return this.date.getTime();
        },
        dateDate() {
            return ("0" + this.date.getDate()).slice(-2) + '.' + ("0" + (this.date.getMonth() + 1)).slice(-2) + '.' + this.date.getFullYear();
        },
        dateTime() {
            return ("0" + this.date.getHours()).slice(-2) + ':' + ("0" + this.date.getMinutes()).slice(-2);
        }
    },
    mounted(){
        if(this.block.data.type == 'countdown') {
            this.countdown();
        }
    },
    methods: {
        countdown() {
            let view = this;
            setInterval(function() {
                let now = new Date().getTime();
                let distance = view.timestamp - now;
                if(distance < 0) {return;}
                view.countdownDays = Math.floor(distance / (1000 * 60 * 60 * 24));
                view.countdownHours = ("0" + Math.floor((distance % (1000 * 60 * 60 * 24)) / (1000 * 60 * 60))).slice(-2);
                view.countdownMinutes = ("0" + Math.floor((distance % (1000 * 60 * 60)) / (1000 * 60))).slice(-2);
                view.countdownSeconds = ("0" + Math.floor((distance % (1000 * 60)) / 1000)).slice(-2);
            }, 1000);
        },
        download() {
            //TODO clean this up
            let saveAs = saveAs||function(e){"use strict";if(typeof e==="undefined"||typeof navigator!=="undefined"&&/MSIE [1-9]\./.test(navigator.userAgent)){return}var t=e.document,n=function(){return e.URL||e.webkitURL||e},r=t.createElementNS("http://www.w3.org/1999/xhtml","a"),o="download"in r,a=function(e){var t=new MouseEvent("click");e.dispatchEvent(t)},i=/constructor/i.test(e.HTMLElement)||e.safari,f=/CriOS\/[\d]+/.test(navigator.userAgent),u=function(t){(e.setImmediate||e.setTimeout)(function(){throw t},0)},s="application/octet-stream",d=1e3*40,c=function(e){var t=function(){if(typeof e==="string"){n().revokeObjectURL(e)}else{e.remove()}};setTimeout(t,d)},l=function(e,t,n){t=[].concat(t);var r=t.length;while(r--){var o=e["on"+t[r]];if(typeof o==="function"){try{o.call(e,n||e)}catch(a){u(a)}}}},p=function(e){if(/^\s*(?:text\/\S*|application\/xml|\S*\/\S*\+xml)\s*;.*charset\s*=\s*utf-8/i.test(e.type)){return new Blob([String.fromCharCode(65279),e],{type:e.type})}return e},v=function(t,u,d){if(!d){t=p(t)}var v=this,w=t.type,m=w===s,y,h=function(){l(v,"writestart progress write writeend".split(" "))},S=function(){if((f||m&&i)&&e.FileReader){var r=new FileReader;r.onloadend=function(){var t=f?r.result:r.result.replace(/^data:[^;]*;/,"data:attachment/file;");var n=e.open(t,"_blank");if(!n)e.location.href=t;t=undefined;v.readyState=v.DONE;h()};r.readAsDataURL(t);v.readyState=v.INIT;return}if(!y){y=n().createObjectURL(t)}if(m){e.location.href=y}else{var o=e.open(y,"_blank");if(!o){e.location.href=y}}v.readyState=v.DONE;h();c(y)};v.readyState=v.INIT;if(o){y=n().createObjectURL(t);setTimeout(function(){r.href=y;r.download=u;a(r);h();c(y);v.readyState=v.DONE});return}S()},w=v.prototype,m=function(e,t,n){return new v(e,t||e.name||"download",n)};if(typeof navigator!=="undefined"&&navigator.msSaveOrOpenBlob){return function(e,t,n){t=t||e.name||"download";if(!n){e=p(e)}return navigator.msSaveOrOpenBlob(e,t)}}w.abort=function(){};w.readyState=w.INIT=0;w.WRITING=1;w.DONE=2;w.error=w.onwritestart=w.onprogress=w.onwrite=w.onabort=w.onerror=w.onwriteend=null;return m}(typeof self!=="undefined"&&self||typeof window!=="undefined"&&window||this.content);if(typeof module!=="undefined"&&module.exports){module.exports.saveAs=saveAs}else if(typeof define!=="undefined"&&define!==null&&define.amd!==null){define("FileSaver.js",function(){return saveAs})}
            let ics = function(e,t){"use strict";{if(!(navigator.userAgent.indexOf("MSIE")>-1&&-1==navigator.userAgent.indexOf("MSIE 10"))){void 0===e&&(e="default"),void 0===t&&(t="Calendar");var r=-1!==navigator.appVersion.indexOf("Win")?"\r\n":"\n",n=[],i=["BEGIN:VCALENDAR","PRODID:"+t,"VERSION:2.0"].join(r),o=r+"END:VCALENDAR",a=["SU","MO","TU","WE","TH","FR","SA"];return{events:function(){return n},calendar:function(){return i+r+n.join(r)+o},addEvent:function(t,i,o,l,u,s){if(void 0===t||void 0===i||void 0===o||void 0===l||void 0===u)return!1;if(s&&!s.rrule){if("YEARLY"!==s.freq&&"MONTHLY"!==s.freq&&"WEEKLY"!==s.freq&&"DAILY"!==s.freq)throw"Recurrence rrule frequency must be provided and be one of the following: 'YEARLY', 'MONTHLY', 'WEEKLY', or 'DAILY'";if(s.until&&isNaN(Date.parse(s.until)))throw"Recurrence rrule 'until' must be a valid date string";if(s.interval&&isNaN(parseInt(s.interval)))throw"Recurrence rrule 'interval' must be an integer";if(s.count&&isNaN(parseInt(s.count)))throw"Recurrence rrule 'count' must be an integer";if(void 0!==s.byday){if("[object Array]"!==Object.prototype.toString.call(s.byday))throw"Recurrence rrule 'byday' must be an array";if(s.byday.length>7)throw"Recurrence rrule 'byday' array must not be longer than the 7 days in a week";s.byday=s.byday.filter(function(e,t){return s.byday.indexOf(e)==t});for(var c in s.byday)if(a.indexOf(s.byday[c])<0)throw"Recurrence rrule 'byday' values must include only the following: 'SU', 'MO', 'TU', 'WE', 'TH', 'FR', 'SA'"}}var g=new Date(l),d=new Date(u),f=new Date,S=("0000"+g.getFullYear().toString()).slice(-4),E=("00"+(g.getMonth()+1).toString()).slice(-2),v=("00"+g.getDate().toString()).slice(-2),y=("00"+g.getHours().toString()).slice(-2),A=("00"+g.getMinutes().toString()).slice(-2),T=("00"+g.getSeconds().toString()).slice(-2),b=("0000"+d.getFullYear().toString()).slice(-4),D=("00"+(d.getMonth()+1).toString()).slice(-2),N=("00"+d.getDate().toString()).slice(-2),h=("00"+d.getHours().toString()).slice(-2),I=("00"+d.getMinutes().toString()).slice(-2),R=("00"+d.getMinutes().toString()).slice(-2),M=("0000"+f.getFullYear().toString()).slice(-4),w=("00"+(f.getMonth()+1).toString()).slice(-2),L=("00"+f.getDate().toString()).slice(-2),O=("00"+f.getHours().toString()).slice(-2),p=("00"+f.getMinutes().toString()).slice(-2),Y=("00"+f.getMinutes().toString()).slice(-2),U="",V="";y+A+T+h+I+R!=0&&(U="T"+y+A+T,V="T"+h+I+R);var B,C=S+E+v+U,j=b+D+N+V,m=M+w+L+("T"+O+p+Y);if(s)if(s.rrule)B=s.rrule;else{if(B="rrule:FREQ="+s.freq,s.until){var x=new Date(Date.parse(s.until)).toISOString();B+=";UNTIL="+x.substring(0,x.length-13).replace(/[-]/g,"")+"000000Z"}s.interval&&(B+=";INTERVAL="+s.interval),s.count&&(B+=";COUNT="+s.count),s.byday&&s.byday.length>0&&(B+=";BYDAY="+s.byday.join(","))}(new Date).toISOString();var H=["BEGIN:VEVENT","UID:"+n.length+"@"+e,"CLASS:PUBLIC","DESCRIPTION:"+i,"DTSTAMP;VALUE=DATE-TIME:"+m,"DTSTART;VALUE=DATE-TIME:"+C,"DTEND;VALUE=DATE-TIME:"+j,"LOCATION:"+o,"SUMMARY;LANGUAGE=de-de:"+t,"TRANSP:TRANSPARENT","END:VEVENT"];return B&&H.splice(4,0,B),H=H.join(r),n.push(H),H},download:function(e,t){if(n.length<1)return!1;t=void 0!==t?t:".ics",e=void 0!==e?e:"calendar";var a,l=i+r+n.join(r)+o;if(-1===navigator.userAgent.indexOf("MSIE 10"))a=new Blob([l]);else{var u=new BlobBuilder;u.append(l),a=u.getBlob("text/x-vCalendar;charset="+document.characterSet)}return saveAs(a,e+t),l},build:function(){return!(n.length<1)&&i+r+n.join(r)+o}}}console.log("Unsupported Browser")}};
            let cal = ics();
            let date = this.block.data.date + " "+ this.block.data.time;

            cal.addEvent(this.block.data.title, this.block.data.description, this.block.data.location, date, date);
            cal.download(this.block.data.title.replace(' ', '_'));
        }
    }
}
</script>

<style lang="less">
.DateBlock {
    display: table;
    margin: 0 auto;
    cursor: pointer;
    .cw-date-countdown {
        .cw-date-countdown-digit {
            display: inline-block;
            margin-right: 4px;
        
            .cw-date-countdown-number {
                font-size: 6em;
                line-height: 1.25em;
                height: 1.25em;
                padding: 3px 29px;
                background: rgba(245,245,245,1);
                font-weight: 300;
            }
        
            .cw-date-countdown-label-sg,
            .cw-date-countdown-label-pl {
                padding: 5px;
                font-size: 1.25em;
                text-align: left;
                text-transform: uppercase;
            }
        }
    }

    .cw-date-date {
        .cw-date-date-space {
            display: inline-block;
            width: 2em;

        }

        .cw-date-date-digits{
            display: inline-block;

            .cw-date-date-number {
                font-size: 5em;
                line-height: 1.25em;
                height: 1.25em;
                padding: 0.25em;
                background: rgba(245,245,245,1);
                font-weight: 300;
            }
        }
    }
}
</style>
