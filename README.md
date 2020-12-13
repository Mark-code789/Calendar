# Calendar
This is a simple web file created to display Calendar visually using the HTML design. 

The code majorly uses HTML table and JavaScript Date object
The code is structured in new ES6 class object. 
Have not separated the individual files. Hence, its just a single HTML file with the CSS and JS files included using the style and script tags. 

Have not included comments in the code since the variables and code is simple to understand. 

here is the code 
<code>
&lt;!DOCTYPE html&gt;
&lt;html&gt;
	   &lt;head&gt;
		<meta charset="UTF-8">
		<meta name="viewport" content="width=device-width, initial-scale=1">
		<title>Calendar</title>
		<style>
			:root {
				--x: -200%;
			} 
			body {
			    height: 100vh;
			    width: 100vw;
			    padding: 8px;
			    margin: 0;
				background: #AAA;
				
				display: grid;
				grid-template-columns: 100%;
				grid-template-rows: 40px 30px 70px auto;
			}
			
			* {
				font-family: serif;
				box-sizing: border-box;
			} 
			
			h1, h2 {
				text-align: center;
				padding: 0;
				margin: 0;
				font-family: serif;
				background: #444;
				color: #DDD;
				text-shadow: 0 2px #000;
			} 
			
			h2 {
				box-shadow: 0 5px 5px 0 #222;
			} 
			
			.year_input {
				height: 30px;
				display: grid;
				grid-column-gap: 5px;
				grid-template-columns: 60px auto 60px;
				grid-template-rows: auto;
			} 
			
			label {
				display: flex;
				justify-content: center;
				align-items: center;
				font-weight: 700;
				padding: 2.5px 10px;
				margin: 0;
				background: #444;
				color: #DDD;
				box-shadow: 0 5px 5px 0 #222;
			} 
			
			input {
				padding: 2.5px 0 2.5px 10px;
				outline: none;
				border: none;
				border-bottom: 1px solid #000;
				box-shadow: 0 5px 5px 0 #222;
				background: #444;
				font-size: 1em;
				font-weight: 500;
				color: #DDD;
			} 
			
			button {
				font-size: 1em;
				font-weight: 500;
				padding: 2.5px 10px;
				background: #444;
				color: #DDD;
				box-shadow: 0 5px 5px 0 #222;
				border: none;
				outline: none;
			} 
			
			.click {
				color: #080;
			} 
			
			article {
				position: relative;
				height: 100%;
				width: 100%;
				padding: 0;
				margin: 0;
				box-shadow: 0 -5px 5px 0 #222;
				overflow-y: auto;
				overflow-x: hidden;
			} 
			
			.calendar {
				position: absolute;
				top: 0;
				left: 0;
				height: 100%;
				width: 100%;
				padding: 0;
				margin: 0;
				background: #444;
				overflow-y: auto;
				overflow-x: hidden;
				
				display: flex;
				justify-content: center;
				align-items: flex-start;
				flex-flow: row wrap;
			} 
			
			table {
			    width: 100%;
				max-width: 400px;
			    border-collapse: collapse;
			    border-spacing: 0;
			    background: #DDD;
				box-shadow: 0 5px 5px 0 #222; 
				margin: 0 10px 20px 10px;
			}
			
			tr {
			    height: calc((100vw - 16px) / 14);
			    border-bottom: 1px solid #000;
			}
			
			td {
			    text-align: center;
			}
			
			td div {
				background: transparent;
			} 
			
			.first_day_of_the_week {
			    color: #800;
			}
			
			.today {
				margin: auto;
				height: 1.5em;
				width: 1.5em;
				background: #888;
				border-radius: 50%;
				display: flex;
				justify-content: center;
				align-items: center;
			} 
			
			.shift_top {
				animation: shift_A 1.5s ease 0s 1 normal forwards;
			} 
			@keyframes shift_A {
				0%{transform: scale(1, 1)}
				25%{transform: scale(0.8, 0.8)}
				100%{transform: scale(0.8, 0.8) translate3d(var(--x), 0, 0)}
			} 
			
			.shift_bottom {
				animation: shift_B 0.75s ease 1s 1 normal forwards;
			} 
			@keyframes shift_B {
				from{transform: scale(0.8, 0.8)}
				to{transform: scale(1, 1)}
			} 
		</style>
	</head>
	<body>
		<h1>SOFT CALENDAR</h1>
		<h2>2020</h2>
		<form onsubmit="View(event)">
			<p class="year_input">
				<label>YEAR:</label>
				<input type="number" min="1970" max="9999" placeholder="2020" id="year" required>
				<button type="submit" onclick="View(event)">VIEW</button>
			</p>
		</form>
		<article>
			<section class="calendar bottom" onanimationend="Switch(event)"></section>
			<section class="calendar top" onanimationend="Switch(event)"></section>
		</article>
		<script>
			class Calendar {
			    constructor (year, calendar) {
			        this.year = year;
					this.calendar = calendar;
			    }
			    
			    fill(month, index) {
					let dateObj = new Date(this.year, index, 1);
					
					let table = $$$("table");
					//table.id = `${month.toLowerCase()}${this.year}`;
					let thead = $$$("thead");
					let tbody = $$$("tbody");
					let tr = thead.insertRow(-1);
					let th = $$$("th");
					tr.appendChild(th);
					th.setAttribute("colspan", "7");
					th.innerHTML = `${month.toUpperCase()} ${this.year}`;
					tr = thead.insertRow(-1);
					
					let days = ["SUN", "MON", "TUE", "WED", "THU", "FRI", "SAT"];
					for(let day of days) {
						th = $$$("th");
						tr.appendChild(th);
						if(day === "SUN") th.classList.add("first_day_of_the_week");
						th.innerHTML = `${day}`;
					} 
					table.appendChild(thead);
					table.appendChild(tbody);
					this.calendar.appendChild(table);
					
					let date = 1;
					let day = dateObj.getDay();
					let currDateObj = new Date();
					
					for(let week = 1;; week++) {
						tr = (week <= 5)? tbody.insertRow(-1): tbody.children[0];
						for(let j = 0; j < 7; j++) {
							let td = (week <= 5)? tr.insertCell(j): tr.cells[j];
							if(j === 0) td.classList.add("first_day_of_the_week");
							if(j >= day && date <= this.daysInAMonth(index)) {
								td.innerHTML = `<div>${date}</div>`;
								if(date === currDateObj.getDate() && index === currDateObj.getMonth() && this.year === currDateObj.getFullYear())
								td.children[0].classList.add("today");
								date++;
							} 
						} 
						if(date > this.daysInAMonth(index)) break;
						day = 0;
					} 
			    }
			
				display() {
					let months = ["JANUARY", "FEBRUARY", "MARCH", "APRIL", "MAY", "JUNE", "JULY", "AUGUST", "SEPTEMBER", "OCTOBER", "NOVEMBER", "DECEMBER"];
					months.forEach((month, index) => {
				    	this.fill(month, index);
					});
				} 
			
				daysInAMonth(month) {
					return new Date(this.year, month+1, 0).getDate();
				} 
			}
			
			function $(elem) {
			    return document.querySelector(elem);
			}
			function $$(elem) {
			    return document.querySelectorAll(elem);
			}
			function $$$(elem) {
				return document.createElement(elem);
			} 
			
			function View(event) {
				let year = parseInt($("#year").value != ""? $("#year").value: 0);
				if(!event.i && (year < 1970 || year > 9999))
					return;
				if(!event.i) {
					document.documentElement.style.setProperty("--x", year - parseInt($("h2").innerHTML) > 0? "-200%": "200%");
					event.preventDefault();
					if(year - parseInt($("h2").innerHTML) === 0) {
						$("#year").blur();
						return;
					} 
				} 
				else {
					document.documentElement.style.setProperty("--x", event.i > 0? "-200%": "200%");
					year = parseInt($("h2").innerHTML) + event.i;
					$("#year").value = year;
				} 
				try {
					$("#year").blur();
					$("h2").innerHTML = year;
					let calendarA = $(".top");
					calendarA.classList.remove("shift_top", "shift_bottom");
					void calendarA.offsetWidth;
					let calendarB = $(".bottom");
					calendarB.classList.remove("shift_top", "shift_bottom");
					void calendarA.offsetWidth;
					calendarB.innerHTML = "";
					calendarB.style.transform = "scale(0.8, 0.8)";
					let calendarObj = new Calendar(year, calendarB);
					calendarObj.display();
					calendarA.classList.add("shift_top");
					calendarB.classList.add("shift_bottom");
				} 
				catch (error) {}
			} 
			
			function Switch (event) {
				if(event.animationName === "shift_A") {
					event.target.className = event.target.className.replace("top", "bottom");
					event.target.style.zIndex = "0";
				} 
				else {
					event.target.className = event.target.className.replace("bottom", "top");
					event.target.style.zIndex = "1";
				} 
			} 
			
			var x1 = null, y1 = null, dx = null, dy = null;
			function TouchStart(event) {
				x1 = event.touches[0].clientX;
				y1 = event.touches[0].clientY;
			} 
			
			function TouchMove(event) {
				let x2 = event.touches[0].clientX;
				let y2 = event.touches[0].clientY;
				dx = x1 - x2;
				dy = y1 - y2;
			} 
			
			function TouchEnd(event) {
				if(event.type == "touchend" && Math.abs(dx) > Math.abs(dy) && dx < 0 || event.type == "keyup" && event.key == "ArrowLeft") {
					View({i: -1});
				} 
				else if(event.type == "touchend" && Math.abs(dx) > Math.abs(dy) && dx > 0 || event.type == "keyup" && event.key == "ArrowRight") {
					View({i: 1});
				} 
				dx = 0;
				dy = 0;
				x1 = 0;
				y1 = 0;
			} 
			
			window.onload = () => {
				document.body.addEventListener("touchstart", TouchStart, true);
				document.body.addEventListener("touchmove", TouchMove, true);
				document.body.addEventListener("touchend", TouchEnd, true);
				document.body.addEventListener("keyup", TouchEnd, true);
			    let calendarObj = new Calendar(new Date().getFullYear(), $(".top"));
				calendarObj.display();
			}
		</script>
	</body>
</html>
</code>
