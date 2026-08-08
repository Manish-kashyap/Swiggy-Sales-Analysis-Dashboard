<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Swiggy Sales Analytics Dashboard</title>

<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>

<style>
*{
    margin:0;
    padding:0;
    box-sizing:border-box;
    font-family:"Segoe UI",Arial,sans-serif;
}

:root{
    --swiggy:#fc8019;
    --orange:#ff5a00;
    --dark:#101820;
    --card:#ffffff;
    --text:#252525;
    --muted:#777;
    --bg:#fff5eb;
}

body{
    min-height:100vh;
    background:
        radial-gradient(circle at 10% 10%,rgba(252,128,25,.18),transparent 25%),
        radial-gradient(circle at 90% 90%,rgba(255,90,0,.12),transparent 25%),
        var(--bg);
    color:var(--text);
    overflow-x:hidden;
}

/* Floating background */
body::before,
body::after{
    content:"";
    position:fixed;
    width:260px;
    height:260px;
    border-radius:50%;
    filter:blur(70px);
    opacity:.25;
    z-index:-1;
    animation:float 8s ease-in-out infinite alternate;
}

body::before{
    background:#fc8019;
    top:-80px;
    left:-70px;
}

body::after{
    background:#ffb347;
    bottom:-100px;
    right:-70px;
    animation-delay:2s;
}

@keyframes float{
    from{transform:translate(0,0) scale(1);}
    to{transform:translate(35px,25px) scale(1.15);}
}

.dashboard{
    width:96%;
    max-width:1500px;
    margin:25px auto;
    display:grid;
    grid-template-columns:230px 1fr;
    gap:20px;
}

/* Sidebar */
.sidebar{
    background:linear-gradient(180deg,#071b2a,#123b55);
    border-radius:25px;
    padding:22px 16px;
    min-height:calc(100vh - 50px);
    box-shadow:0 20px 45px rgba(0,0,0,.2);
    position:sticky;
    top:20px;
    overflow:hidden;
}

.sidebar::before{
    content:"";
    position:absolute;
    width:180px;
    height:180px;
    background:#fc8019;
    border-radius:50%;
    top:-100px;
    right:-100px;
    opacity:.25;
}

.logo{
    display:flex;
    align-items:center;
    gap:10px;
    color:white;
    font-size:27px;
    font-weight:800;
    margin-bottom:35px;
}

.logo-icon{
    width:45px;
    height:45px;
    background:linear-gradient(135deg,#ff9d3d,#fc8019);
    border-radius:14px;
    display:grid;
    place-items:center;
    font-size:25px;
    box-shadow:0 8px 20px rgba(252,128,25,.35);
}

.nav{
    display:flex;
    flex-direction:column;
    gap:12px;
}

.nav a{
    text-decoration:none;
    color:white;
    padding:15px;
    border-radius:14px;
    font-weight:700;
    transition:.3s;
    position:relative;
    overflow:hidden;
}

.nav a::after{
    content:"";
    position:absolute;
    width:0;
    height:100%;
    top:0;
    left:0;
    background:rgba(255,255,255,.12);
    transition:.3s;
}

.nav a:hover::after{
    width:100%;
}

.nav a:hover,
.nav a.active{
    background:linear-gradient(135deg,#ff8a28,#fc8019);
    transform:translateX(6px);
    box-shadow:0 8px 20px rgba(252,128,25,.3);
}

/* Main */
.main{
    min-width:0;
}

.header{
    display:flex;
    justify-content:space-between;
    align-items:center;
    margin-bottom:20px;
    animation:slideDown .7s ease;
}

.title h1{
    font-size:32px;
    font-weight:800;
    color:#122333;
}

.title p{
    color:var(--muted);
    margin-top:4px;
}

.live{
    display:flex;
    align-items:center;
    gap:8px;
    background:white;
    padding:10px 16px;
    border-radius:30px;
    box-shadow:0 8px 25px rgba(0,0,0,.08);
    font-size:13px;
    font-weight:700;
}

.dot{
    width:9px;
    height:9px;
    background:#20c997;
    border-radius:50%;
    animation:pulse 1.5s infinite;
}

@keyframes pulse{
    0%,100%{box-shadow:0 0 0 0 rgba(32,201,151,.5);}
    50%{box-shadow:0 0 0 8px rgba(32,201,151,0);}
}

@keyframes slideDown{
    from{opacity:0;transform:translateY(-20px);}
    to{opacity:1;transform:translateY(0);}
}

/* KPI */
.kpis{
    display:grid;
    grid-template-columns:repeat(5,1fr);
    gap:15px;
    margin-bottom:18px;
}

.kpi{
    background:rgba(255,255,255,.88);
    backdrop-filter:blur(15px);
    border:1px solid rgba(255,255,255,.8);
    border-radius:20px;
    padding:18px;
    position:relative;
    overflow:hidden;
    box-shadow:0 12px 30px rgba(0,0,0,.08);
    animation:cardIn .7s ease backwards;
    transition:.35s;
}

.kpi:nth-child(2){animation-delay:.1s}
.kpi:nth-child(3){animation-delay:.2s}
.kpi:nth-child(4){animation-delay:.3s}
.kpi:nth-child(5){animation-delay:.4s}

@keyframes cardIn{
    from{opacity:0;transform:translateY(30px) scale(.95);}
    to{opacity:1;transform:translateY(0) scale(1);}
}

.kpi:hover{
    transform:translateY(-7px);
    box-shadow:0 20px 40px rgba(252,128,25,.18);
}

.kpi::before{
    content:"";
    position:absolute;
    width:90px;
    height:90px;
    border-radius:50%;
    background:rgba(252,128,25,.08);
    right:-30px;
    top:-30px;
}

.kpi-top{
    display:flex;
    justify-content:space-between;
    align-items:center;
    color:#555;
    font-size:13px;
    font-weight:600;
}

.kpi-icon{
    width:34px;
    height:34px;
    display:grid;
    place-items:center;
    background:#fff0e2;
    border-radius:10px;
    font-size:18px;
}

.kpi h2{
    font-size:26px;
    margin-top:9px;
    color:#151515;
}

.kpi small{
    color:#20a36a;
    font-weight:700;
}

/* Charts */
.grid{
    display:grid;
    grid-template-columns:1.35fr 1fr 1fr;
    gap:18px;
}

.card{
    background:rgba(255,255,255,.9);
    backdrop-filter:blur(15px);
    border-radius:20px;
    padding:18px;
    box-shadow:0 12px 30px rgba(0,0,0,.07);
    border:1px solid rgba(255,255,255,.9);
    animation:fadeUp .8s ease backwards;
    transition:.35s;
}

.card:hover{
    transform:translateY(-4px);
    box-shadow:0 18px 40px rgba(0,0,0,.1);
}

.card h3{
    font-size:15px;
    margin-bottom:15px;
}

.card h3 span{
    color:#fc8019;
}

.chart-large{
    grid-column:span 2;
}

.chart-wide{
    grid-column:span 3;
}

@keyframes fadeUp{
    from{opacity:0;transform:translateY(25px);}
    to{opacity:1;transform:translateY(0);}
}

.chart-box{
    height:260px;
}

.chart-small{
    height:230px;
}

/* Insight cards */
.insights{
    display:grid;
    grid-template-columns:repeat(3,1fr);
    gap:15px;
    margin-top:18px;
}

.insight{
    background:linear-gradient(135deg,#fff,#fff4e9);
    border-left:5px solid #fc8019;
    border-radius:16px;
    padding:17px;
    box-shadow:0 8px 22px rgba(0,0,0,.06);
}

.insight strong{
    display:block;
    color:#fc8019;
    margin-bottom:6px;
}

.insight p{
    font-size:13px;
    line-height:1.5;
    color:#666;
}

/* Responsive */
@media(max-width:1100px){
    .dashboard{
        grid-template-columns:1fr;
    }

    .sidebar{
        position:relative;
        min-height:auto;
    }

    .nav{
        flex-direction:row;
        flex-wrap:wrap;
    }

    .kpis{
        grid-template-columns:repeat(3,1fr);
    }

    .grid{
        grid-template-columns:1fr 1fr;
    }

    .chart-large,
    .chart-wide{
        grid-column:span 2;
    }
}

@media(max-width:700px){
    .dashboard{
        width:94%;
        margin:15px auto;
    }

    .kpis{
        grid-template-columns:1fr 1fr;
    }

    .grid,
    .insights{
        grid-template-columns:1fr;
    }

    .chart-large,
    .chart-wide{
        grid-column:span 1;
    }

    .header{
        align-items:flex-start;
        gap:12px;
        flex-direction:column;
    }

    .title h1{
        font-size:25px;
    }
}
</style>
</head>

<body>

<div class="dashboard">

    <!-- SIDEBAR -->
    <aside class="sidebar">

        <div class="logo">
            <div class="logo-icon">S</div>
            Swiggy
        </div>

        <nav class="nav">
            <a href="#" class="active">📊 Dashboard</a>
            <a href="#analysis">📈 Analysis</a>
            <a href="#insights">💡 Insights</a>
            <a href="#data">🗄️ Data</a>
        </nav>

    </aside>


    <!-- MAIN CONTENT -->
    <main class="main">

        <header class="header">
            <div class="title">
                <h1>Swiggy Sales Analytics</h1>
                <p>Food Delivery Business Intelligence Dashboard</p>
            </div>

            <div class="live">
                <span class="dot"></span>
                Live Analytics
            </div>
        </header>


        <!-- KPI CARDS -->
        <section class="kpis">

            <div class="kpi">
                <div class="kpi-top">
                    Total Sales
                    <div class="kpi-icon">💰</div>
                </div>
                <h2>₹53.01M</h2>
                <small>▲ Sales Performance</small>
            </div>

            <div class="kpi">
                <div class="kpi-top">
                    Average Rating
                    <div class="kpi-icon">⭐</div>
                </div>
                <h2>4.34</h2>
                <small>Excellent Rating</small>
            </div>

            <div class="kpi">
                <div class="kpi-top">
                    Avg Order Value
                    <div class="kpi-icon">🛒</div>
                </div>
                <h2>₹268.51</h2>
                <small>Per Transaction</small>
            </div>

            <div class="kpi">
                <div class="kpi-top">
                    Rating Count
                    <div class="kpi-icon">👍</div>
                </div>
                <h2>5.59M</h2>
                <small>Customer Ratings</small>
            </div>

            <div class="kpi">
                <div class="kpi-top">
                    Total Orders
                    <div class="kpi-icon">📦</div>
                </div>
                <h2>197.43K</h2>
                <small>Transactions</small>
            </div>

        </section>


        <!-- CHARTS -->
        <section class="grid" id="analysis">

            <div class="card chart-large">
                <h3><span>Monthly</span> Sales Trend</h3>
                <div class="chart-box">
                    <canvas id="monthlyChart"></canvas>
                </div>
            </div>


            <div class="card">
                <h3><span>Sales</span> by Food Type</h3>
                <div class="chart-small">
                    <canvas id="foodChart"></canvas>
                </div>
            </div>


            <div class="card">
                <h3><span>Daily</span> Sales Trend</h3>
                <div class="chart-small">
                    <canvas id="dailyChart"></canvas>
                </div>
            </div>


            <div class="card">
                <h3><span>Quarterly</span> Performance</h3>
                <div class="chart-small">
                    <canvas id="quarterChart"></canvas>
                </div>
            </div>


            <div class="card">
                <h3><span>Top 5</span> Cities by Sales</h3>
                <div class="chart-small">
                    <canvas id="cityChart"></canvas>
                </div>
            </div>


            <div class="card chart-wide">
                <h3><span>Weekly</span> Sales Trend</h3>
                <div class="chart-box">
                    <canvas id="weeklyChart"></canvas>
                </div>
            </div>

        </section>


        <!-- INSIGHTS -->
        <section class="insights" id="insights">

            <div class="insight">
                <strong>🏆 Top City</strong>
                <p>Bengaluru leads the Top 5 cities with approximately ₹5.46M in sales.</p>
            </div>

            <div class="insight">
                <strong>🥗 Food Preference</strong>
                <p>Veg food contributes approximately 64% of total food-type sales.</p>
            </div>

            <div class="insight">
                <strong>📅 Best Day</strong>
                <p>Saturday records the highest daily sales at approximately ₹7.78M.</p>
            </div>

        </section>

    </main>

</div>


<script>

Chart.defaults.font.family = "Segoe UI";
Chart.defaults.color = "#666";
Chart.defaults.animation.duration = 1400;
Chart.defaults.animation.easing = "easeOutQuart";

const orange = "#fc8019";
const dark = "#123b55";
const lightOrange = "rgba(252,128,25,.18)";


/* MONTHLY SALES */
new Chart(document.getElementById("monthlyChart"),{
    type:"line",
    data:{
        labels:["Jan","Feb","Mar","Apr","May","Jun","Jul","Aug"],
        datasets:[{
            label:"Sales",
            data:[6.80,6.25,6.58,6.60,6.77,6.52,6.63,6.75],
            borderColor:orange,
            backgroundColor:lightOrange,
            fill:true,
            tension:.45,
            pointRadius:5,
            pointHoverRadius:9,
            pointBackgroundColor:orange
        }]
    },
    options:{
        responsive:true,
        maintainAspectRatio:false,
        plugins:{
            legend:{display:false}
        },
        scales:{
            y:{
                ticks:{
                    callback:value=>"₹"+value+"M"
                },
                grid:{color:"rgba(0,0,0,.05)"}
            },
            x:{
                grid:{display:false}
            }
        }
    }
});


/* FOOD TYPE */
new Chart(document.getElementById("foodChart"),{
    type:"doughnut",
    data:{
        labels:["Veg","Non-Veg"],
        datasets:[{
            data:[34.2,18.8],
            backgroundColor:[orange,dark],
            borderWidth:5,
            borderColor:"#fff",
            hoverOffset:12
        }]
    },
    options:{
        responsive:true,
        maintainAspectRatio:false,
        cutout:"68%",
        plugins:{
            legend:{
                position:"bottom"
            }
        }
    }
});


/* DAILY SALES */
new Chart(document.getElementById("dailyChart"),{
    type:"bar",
    data:{
        labels:["Mon","Tue","Wed","Thu","Fri","Sat","Sun"],
        datasets:[{
            label:"Sales",
            data:[7.45,7.36,7.54,7.66,7.58,7.78,7.64],
            backgroundColor:orange,
            borderRadius:8,
            hoverBackgroundColor:dark
        }]
    },
    options:{
        responsive:true,
        maintainAspectRatio:false,
        plugins:{
            legend:{display:false}
        },
        scales:{
            y:{
                ticks:{
                    callback:value=>"₹"+value+"M"
                },
                grid:{color:"rgba(0,0,0,.05)"}
            },
            x:{grid:{display:false}}
        }
    }
});


/* QUARTER */
new Chart(document.getElementById("quarterChart"),{
    type:"bar",
    data:{
        labels:["Q1","Q2","Q3"],
        datasets:[
            {
                label:"Sales (M)",
                data:[19.7,19.9,13.4],
                backgroundColor:orange,
                borderRadius:7
            },
            {
                label:"Orders (K)",
                data:[73.1,74.2,50.2],
                backgroundColor:dark,
                borderRadius:7
            }
        ]
    },
    options:{
        responsive:true,
        maintainAspectRatio:false,
        plugins:{
            legend:{
                position:"bottom"
            }
        },
        scales:{
            y:{
                grid:{color:"rgba(0,0,0,.05)"}
            },
            x:{grid:{display:false}}
        }
    }
});


/* TOP CITIES */
new Chart(document.getElementById("cityChart"),{
    type:"bar",
    data:{
        labels:["Bengaluru","Lucknow","Hyderabad","Mumbai","New Delhi"],
        datasets:[{
            label:"Sales (M)",
            data:[5.46,3.12,3.02,3.02,2.83],
            backgroundColor:dark,
            borderRadius:8,
            hoverBackgroundColor:orange
        }]
    },
    options:{
        indexAxis:"y",
        responsive:true,
        maintainAspectRatio:false,
        plugins:{
            legend:{display:false}
        },
        scales:{
            x:{
                ticks:{
                    callback:value=>"₹"+value+"M"
                },
                grid:{color:"rgba(0,0,0,.05)"}
            },
            y:{grid:{display:false}}
        }
    }
});


/* WEEKLY SALES */
new Chart(document.getElementById("weeklyChart"),{
    type:"bar",
    data:{
        labels:[
            "1","2","3","4","5","6","7","8","9","10",
            "11","12","13","14","15","16","17","18","19","20",
            "21","22","23","24","25","26","27","28","29","30",
            "31","32","33","34","35","36"
        ],
        datasets:[{
            label:"Weekly Sales",
            data:[
                .82,1.52,1.51,1.50,1.53,1.49,1.51,1.73,1.48,
                1.47,1.49,1.50,1.48,1.52,1.50,1.53,1.51,1.50,
                1.51,1.52,1.50,1.51,1.50,1.49,1.51,1.52,1.50,
                1.48,1.50,1.49,1.51,1.52,1.50,1.49,1.52,.18
            ],
            backgroundColor:orange,
            borderRadius:5,
            hoverBackgroundColor:dark
        }]
    },
    options:{
        responsive:true,
        maintainAspectRatio:false,
        plugins:{
            legend:{display:false}
        },
        scales:{
            y:{
                ticks:{
                    callback:value=>"₹"+value+"M"
                },
                grid:{color:"rgba(0,0,0,.05)"}
            },
            x:{
                grid:{display:false}
            }
        }
    }
});

</script>

</body>
</html>
