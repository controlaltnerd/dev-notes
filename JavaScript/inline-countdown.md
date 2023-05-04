# Inline Countdown

A quick countdown I threw together in vanilla JS. Only variable that should be modified is `deadline`. Plain text in the `p` tag can be edited as needed.

### HTML

```
<p id="text-clock">Only <span class="ucM"></span> months, <span class="ucD"></span> days, <span class="ucH"></span> hours, <span class="ucMin"></span> minutes, and <span class="ucS"></span> seconds left!</p>
```

### JS

```
const deadline = 'December 31 2099 23:59:59 GMT-0400';

const clockWrap = document.getElementById("text-clock");
const clock = {
    months: clockWrap.querySelector('.ucM'),
    days: clockWrap.querySelector('.ucD'),
    hours: clockWrap.querySelector('.ucH'),
    minutes: clockWrap.querySelector('.ucMin'),
    seconds: clockWrap.querySelector('.ucS')
}

let clockIntervId;

if (!clockIntervId) {
  clockIntervId = setInterval(() => runClock(deadline), 1000);
}

function getTimeLeft(end) {
	const timeDiff = Date.parse(end) - Date.parse(new Date());

	return {
    'total': timeDiff,
    'months': Math.floor ( timeDiff/(1000*60*60*24*30) ),
    'days': Math.floor( timeDiff/(1000*60*60*24) % 30 ),
    'hours': Math.floor( (timeDiff/(1000*60*60)) % 24 ),
    'minutes': Math.floor( timeDiff/(1000*60) % 60 ),
    'seconds': Math.floor( timeDiff/(1000) % 60 )
  };

}

function runClock(end) {
  const timeLeft = getTimeLeft(end);

  if (timeLeft.total <= 0) {
    updateClock({
      months: 0,
      days: 0,
      hours: 0,
      minutes: 0,
      seconds: 0
    });
    return;
  }

  updateClock(timeLeft);
}

function updateClock(timeLeft) {
  clock.months.innerHTML = timeLeft.months;
  clock.days.innerHTML = timeLeft.days;
  clock.hours.innerHTML = timeLeft.hours;
  clock.minutes.innerHTML = timeLeft.minutes;
  clock.seconds.innerHTML = timeLeft.seconds;
}
```
