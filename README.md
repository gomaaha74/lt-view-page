<!doctype html>
<html lang="en" class="h-full">
 <head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>EV Bike Management Platform</title>
  <script src="/_sdk/element_sdk.js"></script>
  <script src="https://cdn.tailwindcss.com"></script>
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
  <style>
    body {
      box-sizing: border-box;
      font-family: 'Inter', -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif;
    }
    
    .stat-card {
      transition: all 0.3s ease;
    }
    
    .stat-card:hover {
      transform: translateY(-2px);
      box-shadow: 0 10px 25px rgba(0, 0, 0, 0.15);
    }
    
    .menu-card {
      transition: all 0.3s ease;
      cursor: pointer;
    }
    
    .menu-card:hover {
      transform: translateY(-4px);
      box-shadow: 0 12px 30px rgba(0, 0, 0, 0.2);
    }
    
    .sidebar-item {
      transition: all 0.2s ease;
    }
    
    .sidebar-item:hover {
      background: rgba(255, 255, 255, 0.1);
    }
    
    .sidebar-item.active {
      background: rgba(255, 255, 255, 0.15);
      border-left: 4px solid #fff;
    }
    
    .chart-placeholder {
      background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
      border-radius: 8px;
      display: flex;
      align-items: center;
      justify-content: center;
      color: white;
      font-size: 14px;
      min-height: 200px;
    }
    
    .trend-badge {
      animation: pulse 2s ease-in-out infinite;
    }
    
    @keyframes pulse {
      0%, 100% { opacity: 1; }
      50% { opacity: 0.7; }
    }
    
    .status-online { color: #10b981; }
    .status-offline { color: #ef4444; }
    .status-warning { color: #f59e0b; }
  </style>
  <style>@view-transition { navigation: auto; }</style>
  <script src="/_sdk/data_sdk.js" type="text/javascript"></script>
 </head>
 <body class="h-full">
  <div id="app" class="h-full w-full overflow-auto" style="background: linear-gradient(135deg, #1e3a8a 0%, #3b82f6 100%);"><!-- Sidebar -->
   <div id="sidebar" style="position: fixed; left: 0; top: 0; width: 260px; height: 100%; background: rgba(0, 0, 0, 0.3); backdrop-filter: blur(10px); z-index: 100; overflow-y: auto;">
    <div style="padding: 24px;">
     <div style="color: white; font-size: 24px; font-weight: bold; margin-bottom: 8px;" id="platformName">
      EV Bike Management
     </div>
     <div style="color: rgba(255, 255, 255, 0.7); font-size: 14px;" id="companyName">
      Smart Mobility Solutions
     </div>
    </div>
    <nav style="padding: 0 12px;">
     <div class="sidebar-item active" data-page="home" onclick="window.navigateToPage('home')" style="padding: 12px 16px; border-radius: 8px; color: white; margin-bottom: 4px; display: flex; align-items: center; gap: 12px; cursor: pointer;"><i class="fas fa-home" style="width: 20px;"></i> <span>Home / Overview</span>
     </div>
     <div class="sidebar-item" data-page="asset" onclick="window.navigateToPage('asset')" style="padding: 12px 16px; border-radius: 8px; color: rgba(255, 255, 255, 0.8); margin-bottom: 4px; display: flex; align-items: center; gap: 12px; cursor: pointer;"><i class="fas fa-database" style="width: 20px;"></i> <span>Asset Center</span>
     </div>
     <div class="sidebar-item" data-page="operations" onclick="window.navigateToPage('operations')" style="padding: 12px 16px; border-radius: 8px; color: rgba(255, 255, 255, 0.8); margin-bottom: 4px; display: flex; align-items: center; gap: 12px; cursor: pointer;"><i class="fas fa-chart-line" style="width: 20px;"></i> <span>Operations Center</span>
     </div>
     <div class="sidebar-item" data-page="swap" onclick="window.navigateToPage('swap')" style="padding: 12px 16px; border-radius: 8px; color: rgba(255, 255, 255, 0.8); margin-bottom: 4px; display: flex; align-items: center; gap: 12px; cursor: pointer;"><i class="fas fa-exchange-alt" style="width: 20px;"></i> <span>Battery Swap Events</span>
     </div>
     <div class="sidebar-item" data-page="data" onclick="window.navigateToPage('data')" style="padding: 12px 16px; border-radius: 8px; color: rgba(255, 255, 255, 0.8); margin-bottom: 4px; display: flex; align-items: center; gap: 12px; cursor: pointer;"><i class="fas fa-map-marked-alt" style="width: 20px;"></i> <span>Data Center</span>
     </div>
     <div class="sidebar-item" data-page="vehicles" onclick="window.navigateToPage('vehicles')" style="padding: 12px 16px; border-radius: 8px; color: rgba(255, 255, 255, 0.8); margin-bottom: 4px; display: flex; align-items: center; gap: 12px; cursor: pointer;"><i class="fas fa-motorcycle" style="width: 20px;"></i> <span>Vehicle Statistics</span>
     </div>
     <div class="sidebar-item" data-page="station" onclick="window.navigateToPage('station')" style="padding: 12px 16px; border-radius: 8px; color: rgba(255, 255, 255, 0.8); margin-bottom: 4px; display: flex; align-items: center; gap: 12px; cursor: pointer;"><i class="fas fa-charging-station" style="width: 20px;"></i> <span>Station Analytics</span>
     </div>
    </nav>
   </div><!-- Main Content -->
   <div style="margin-left: 260px; min-height: 100%;"><!-- Home Page -->
    <div id="homePage" class="page-content" style="padding: 32px;">
     <h1 style="color: white; font-size: 32px; font-weight: bold; margin-bottom: 32px;">Dashboard Overview</h1><!-- KPI Cards -->
     <div style="display: grid; grid-template-columns: repeat(auto-fit, minmax(250px, 1fr)); gap: 24px; margin-bottom: 40px;">
      <div class="stat-card" style="background: white; border-radius: 16px; padding: 24px; box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);">
       <div style="display: flex; align-items: center; justify-content: space-between; margin-bottom: 12px;">
        <div style="font-size: 14px; color: #6b7280; font-weight: 500;">
         Operating Revenue
        </div><i class="fas fa-dollar-sign" style="color: #10b981; font-size: 24px;"></i>
       </div>
       <div style="font-size: 32px; font-weight: bold; color: #1f2937;">
        RM 245,890
       </div>
       <div style="font-size: 12px; color: #10b981; margin-top: 8px;"><span class="trend-badge">↑ 12.5%</span> vs last month
       </div>
      </div>
      <div class="stat-card" style="background: white; border-radius: 16px; padding: 24px; box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);">
       <div style="display: flex; align-items: center; justify-content: space-between; margin-bottom: 12px;">
        <div style="font-size: 14px; color: #6b7280; font-weight: 500;">
         Number of Orders
        </div><i class="fas fa-shopping-cart" style="color: #3b82f6; font-size: 24px;"></i>
       </div>
       <div style="font-size: 32px; font-weight: bold; color: #1f2937;">
        1,847
       </div>
       <div style="font-size: 12px; color: #10b981; margin-top: 8px;"><span class="trend-badge">↑ 8.3%</span> vs last month
       </div>
      </div>
      <div class="stat-card" style="background: white; border-radius: 16px; padding: 24px; box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);">
       <div style="display: flex; align-items: center; justify-content: space-between; margin-bottom: 12px;">
        <div style="font-size: 14px; color: #6b7280; font-weight: 500;">
         Total Devices
        </div><i class="fas fa-battery-full" style="color: #f59e0b; font-size: 24px;"></i>
       </div>
       <div style="font-size: 32px; font-weight: bold; color: #1f2937;">
        3,248
       </div>
       <div style="font-size: 12px; color: #10b981; margin-top: 8px;"><span class="trend-badge">↑ 5.2%</span> vs last month
       </div>
      </div>
      <div class="stat-card" style="background: white; border-radius: 16px; padding: 24px; box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);">
       <div style="display: flex; align-items: center; justify-content: space-between; margin-bottom: 12px;">
        <div style="font-size: 14px; color: #6b7280; font-weight: 500;">
         Cumulative Users
        </div><i class="fas fa-users" style="color: #8b5cf6; font-size: 24px;"></i>
       </div>
       <div style="font-size: 32px; font-weight: bold; color: #1f2937;">
        12,458
       </div>
       <div style="font-size: 12px; color: #10b981; margin-top: 8px;"><span class="trend-badge">↑ 15.7%</span> vs last month
       </div>
      </div>
     </div><!-- Menu Cards -->
     <h2 style="color: white; font-size: 24px; font-weight: bold; margin-bottom: 24px;">Quick Access</h2>
     <div style="display: grid; grid-template-columns: repeat(auto-fit, minmax(200px, 1fr)); gap: 20px;">
      <div class="menu-card" data-page="asset" onclick="window.navigateToPage('asset')" style="background: linear-gradient(135deg, #667eea 0%, #764ba2 100%); border-radius: 12px; padding: 24px; text-align: center; color: white;"><i class="fas fa-battery-three-quarters" style="font-size: 36px; margin-bottom: 12px;"></i>
       <div style="font-weight: 600; font-size: 16px;">
        Battery Inventory
       </div>
      </div>
      <div class="menu-card" data-page="vehicles" onclick="window.navigateToPage('vehicles')" style="background: linear-gradient(135deg, #f093fb 0%, #f5576c 100%); border-radius: 12px; padding: 24px; text-align: center; color: white;"><i class="fas fa-motorcycle" style="font-size: 36px; margin-bottom: 12px;"></i>
       <div style="font-weight: 600; font-size: 16px;">
        Vehicle Inventory
       </div>
      </div>
      <div class="menu-card" data-page="vehicles" onclick="window.navigateToPage('vehicles')" style="background: linear-gradient(135deg, #4facfe 0%, #00f2fe 100%); border-radius: 12px; padding: 24px; text-align: center; color: white;"><i class="fas fa-box" style="font-size: 36px; margin-bottom: 12px;"></i>
       <div style="font-weight: 600; font-size: 16px;">
        Vehicle Product
       </div>
      </div>
      <div class="menu-card" data-page="operations" onclick="window.navigateToPage('operations')" style="background: linear-gradient(135deg, #43e97b 0%, #38f9d7 100%); border-radius: 12px; padding: 24px; text-align: center; color: white;"><i class="fas fa-file-invoice" style="font-size: 36px; margin-bottom: 12px;"></i>
       <div style="font-weight: 600; font-size: 16px;">
        Vehicle Order
       </div>
      </div>
      <div class="menu-card" data-page="operations" onclick="window.navigateToPage('operations')" style="background: linear-gradient(135deg, #fa709a 0%, #fee140 100%); border-radius: 12px; padding: 24px; text-align: center; color: white;"><i class="fas fa-store" style="font-size: 36px; margin-bottom: 12px;"></i>
       <div style="font-weight: 600; font-size: 16px;">
        Merchant Manage
       </div>
      </div>
      <div class="menu-card" data-page="data" onclick="window.navigateToPage('data')" style="background: linear-gradient(135deg, #30cfd0 0%, #330867 100%); border-radius: 12px; padding: 24px; text-align: center; color: white;"><i class="fas fa-user-friends" style="font-size: 36px; margin-bottom: 12px;"></i>
       <div style="font-weight: 600; font-size: 16px;">
        User List
       </div>
      </div>
      <div class="menu-card" data-page="operations" onclick="window.navigateToPage('operations')" style="background: linear-gradient(135deg, #a8edea 0%, #fed6e3 100%); border-radius: 12px; padding: 24px; text-align: center; color: white;"><i class="fas fa-receipt" style="font-size: 36px; margin-bottom: 12px;"></i>
       <div style="font-weight: 600; font-size: 16px;">
        Transaction Bill
       </div>
      </div>
     </div>
    </div><!-- Asset Center Page -->
    <div id="assetPage" class="page-content" style="padding: 32px; display: none;">
     <h1 style="color: white; font-size: 32px; font-weight: bold; margin-bottom: 32px;">Asset Center - Data Overview</h1><!-- Total Vehicles -->
     <div style="background: white; border-radius: 16px; padding: 28px; margin-bottom: 24px; box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);">
      <div style="display: flex; align-items: center; gap: 16px;">
       <div style="width: 64px; height: 64px; background: linear-gradient(135deg, #667eea 0%, #764ba2 100%); border-radius: 16px; display: flex; align-items: center; justify-content: center;"><i class="fas fa-motorcycle" style="font-size: 28px; color: white;"></i>
       </div>
       <div>
        <div style="color: #6b7280; font-size: 14px; margin-bottom: 4px; font-weight: 500;">
         Total Number of Vehicles
        </div>
        <div style="font-size: 36px; font-weight: bold; color: #1f2937;">
         1,847
        </div>
        <div style="font-size: 13px; color: #10b981; margin-top: 4px;"><span>↑ 12 new vehicles this week</span>
        </div>
       </div>
      </div>
     </div><!-- Battery Status Trend -->
     <div style="background: white; border-radius: 12px; padding: 24px; margin-bottom: 24px;">
      <div style="display: flex; justify-content: space-between; align-items: center; margin-bottom: 20px;">
       <h3 style="font-size: 18px; font-weight: 600; color: #1f2937;">Battery Status Trend (All Batteries)</h3>
       <div style="display: flex; gap: 8px;"><button onclick="filterBatteryTrend('day')" class="time-filter-btn active" data-filter="day" style="padding: 6px 14px; background: #667eea; color: white; border: none; border-radius: 6px; font-size: 13px; cursor: pointer; font-weight: 500;">Day</button> <button onclick="filterBatteryTrend('week')" class="time-filter-btn" data-filter="week" style="padding: 6px 14px; background: #f3f4f6; color: #6b7280; border: none; border-radius: 6px; font-size: 13px; cursor: pointer; font-weight: 500;">Week</button> <button onclick="filterBatteryTrend('month')" class="time-filter-btn" data-filter="month" style="padding: 6px 14px; background: #f3f4f6; color: #6b7280; border: none; border-radius: 6px; font-size: 13px; cursor: pointer; font-weight: 500;">Month</button> <button onclick="filterBatteryTrend('custom')" class="time-filter-btn" data-filter="custom" style="padding: 6px 14px; background: #f3f4f6; color: #6b7280; border: none; border-radius: 6px; font-size: 13px; cursor: pointer; font-weight: 500;">Custom</button>
       </div>
      </div><!-- Battery Status Cards -->
      <div style="display: grid; grid-template-columns: repeat(auto-fit, minmax(180px, 1fr)); gap: 16px; margin-bottom: 20px;">
       <div style="background: #dbeafe; border-radius: 10px; padding: 16px; border-left: 4px solid #3b82f6;">
        <div style="color: #1e40af; font-size: 13px; margin-bottom: 6px; font-weight: 500;">
         In Transit
        </div>
        <div style="font-size: 28px; font-weight: bold; color: #1e40af;">
         245
        </div>
        <div style="font-size: 12px; color: #3b82f6; margin-top: 4px;">
         8.6% of total
        </div>
       </div>
       <div style="background: #d1fae5; border-radius: 10px; padding: 16px; border-left: 4px solid #10b981;">
        <div style="color: #065f46; font-size: 13px; margin-bottom: 6px; font-weight: 500;">
         In Use
        </div>
        <div style="font-size: 28px; font-weight: bold; color: #065f46;">
         1,547
        </div>
        <div style="font-size: 12px; color: #10b981; margin-top: 4px;">
         54.3% of total
        </div>
       </div>
       <div style="background: #fef3c7; border-radius: 10px; padding: 16px; border-left: 4px solid #f59e0b;">
        <div style="color: #92400e; font-size: 13px; margin-bottom: 6px; font-weight: 500;">
         In Stock
        </div>
        <div style="font-size: 28px; font-weight: bold; color: #92400e;">
         723
        </div>
        <div style="font-size: 12px; color: #f59e0b; margin-top: 4px;">
         25.4% of total
        </div>
       </div>
       <div style="background: #e0e7ff; border-radius: 10px; padding: 16px; border-left: 4px solid #6366f1;">
        <div style="color: #3730a3; font-size: 13px; margin-bottom: 6px; font-weight: 500;">
         In Cabinet
        </div>
        <div style="font-size: 28px; font-weight: bold; color: #3730a3;">
         332
        </div>
        <div style="font-size: 12px; color: #6366f1; margin-top: 4px;">
         11.7% of total
        </div>
       </div>
      </div><!-- Chart Placeholder -->
      <div class="chart-placeholder" style="min-height: 250px;">
       Battery Status Trend Over Time - Line/Area Chart
      </div>
     </div><!-- Pending Alarms Section -->
     <div style="background: white; border-radius: 12px; padding: 24px; margin-bottom: 24px;">
      <h3 style="font-size: 18px; font-weight: 600; margin-bottom: 20px; color: #1f2937;">Pending Alarms (All Devices)</h3>
      <div style="color: #6b7280; font-size: 13px; margin-bottom: 16px;">
       Batteries • Cabinets • Vehicles
      </div>
      <div style="display: grid; grid-template-columns: repeat(auto-fit, minmax(200px, 1fr)); gap: 16px; margin-bottom: 20px;">
       <div style="background: #fef2f2; border-radius: 10px; padding: 20px; border-left: 4px solid #ef4444;">
        <div style="color: #991b1b; font-size: 13px; margin-bottom: 6px; font-weight: 500;">
         Total Alarms
        </div>
        <div style="font-size: 32px; font-weight: bold; color: #991b1b;">
         87
        </div>
       </div>
       <div style="background: #fef3c7; border-radius: 10px; padding: 20px; border-left: 4px solid #f59e0b;">
        <div style="color: #92400e; font-size: 13px; margin-bottom: 6px; font-weight: 500;">
         New Alarms Today
        </div>
        <div style="font-size: 32px; font-weight: bold; color: #92400e;">
         23
        </div>
       </div>
       <div style="background: #d1fae5; border-radius: 10px; padding: 20px; border-left: 4px solid #10b981;">
        <div style="color: #065f46; font-size: 13px; margin-bottom: 6px; font-weight: 500;">
         Resolved Today
        </div>
        <div style="font-size: 32px; font-weight: bold; color: #065f46;">
         15
        </div>
       </div>
      </div><!-- Alarm Breakdown -->
      <div style="background: #f9fafb; border-radius: 8px; padding: 16px; margin-bottom: 16px;">
       <div style="font-size: 14px; font-weight: 600; color: #1f2937; margin-bottom: 12px;">
        Alarm Breakdown by Device Type
       </div>
       <div style="display: flex; flex-direction: column; gap: 10px;">
        <div style="display: flex; justify-content: space-between; align-items: center;">
         <div style="display: flex; align-items: center; gap: 8px;"><i class="fas fa-battery-half" style="color: #ef4444;"></i> <span style="font-size: 14px; color: #374151;">Battery Alarms</span>
         </div><span style="font-weight: 600; color: #1f2937;">42</span>
        </div>
        <div style="display: flex; justify-content: space-between; align-items: center;">
         <div style="display: flex; align-items: center; gap: 8px;"><i class="fas fa-box" style="color: #f59e0b;"></i> <span style="font-size: 14px; color: #374151;">Cabinet Alarms</span>
         </div><span style="font-weight: 600; color: #1f2937;">28</span>
        </div>
        <div style="display: flex; justify-content: space-between; align-items: center;">
         <div style="display: flex; align-items: center; gap: 8px;"><i class="fas fa-motorcycle" style="color: #3b82f6;"></i> <span style="font-size: 14px; color: #374151;">Vehicle Alarms</span>
         </div><span style="font-weight: 600; color: #1f2937;">17</span>
        </div>
       </div>
      </div><button onclick="showAlarmDetails()" style="width: 100%; padding: 12px; background: linear-gradient(135deg, #667eea 0%, #764ba2 100%); color: white; border: none; border-radius: 8px; font-weight: 600; font-size: 14px; cursor: pointer; transition: all 0.3s;" onmouseover="this.style.transform='translateY(-2px)'; this.style.boxShadow='0 4px 12px rgba(102, 126, 234, 0.4)'" onmouseout="this.style.transform='translateY(0)'; this.style.boxShadow='none'"> <i class="fas fa-external-link-alt"></i> Check Details - Go to Alarm Management </button>
     </div><!-- Device Battery Online Trend -->
     <div style="background: white; border-radius: 12px; padding: 24px;">
      <div style="display: flex; justify-content: space-between; align-items: center; margin-bottom: 20px;">
       <div>
        <h3 style="font-size: 18px; font-weight: 600; color: #1f2937; margin-bottom: 4px;">Device Battery Online Trend</h3>
        <div style="font-size: 13px; color: #6b7280;">
         All Devices - Real-time monitoring
        </div>
       </div>
       <div style="display: flex; gap: 16px; align-items: center;">
        <div style="display: flex; align-items: center; gap: 8px;">
         <div style="width: 12px; height: 12px; background: #10b981; border-radius: 50%;"></div><span style="font-size: 13px; color: #6b7280;">Online</span>
        </div>
        <div style="display: flex; align-items: center; gap: 8px;">
         <div style="width: 12px; height: 12px; background: #ef4444; border-radius: 50%;"></div><span style="font-size: 13px; color: #6b7280;">Offline</span>
        </div>
       </div>
      </div><!-- Current Status Summary -->
      <div style="display: grid; grid-template-columns: repeat(auto-fit, minmax(150px, 1fr)); gap: 16px; margin-bottom: 20px;">
       <div style="background: #f0fdf4; border-radius: 8px; padding: 16px; text-align: center;">
        <div style="font-size: 13px; color: #166534; margin-bottom: 6px;">
         Online Now
        </div>
        <div style="font-size: 28px; font-weight: bold; color: #16a34a;">
         3,067
        </div>
        <div style="font-size: 12px; color: #22c55e; margin-top: 4px;">
         94.4% uptime
        </div>
       </div>
       <div style="background: #fef2f2; border-radius: 8px; padding: 16px; text-align: center;">
        <div style="font-size: 13px; color: #991b1b; margin-bottom: 6px;">
         Offline Now
        </div>
        <div style="font-size: 28px; font-weight: bold; color: #dc2626;">
         181
        </div>
        <div style="font-size: 12px; color: #ef4444; margin-top: 4px;">
         5.6% downtime
        </div>
       </div>
       <div style="background: #eff6ff; border-radius: 8px; padding: 16px; text-align: center;">
        <div style="font-size: 13px; color: #1e40af; margin-bottom: 6px;">
         Total Devices
        </div>
        <div style="font-size: 28px; font-weight: bold; color: #2563eb;">
         3,248
        </div>
        <div style="font-size: 12px; color: #3b82f6; margin-top: 4px;">
         All categories
        </div>
       </div>
      </div><!-- Trend Chart -->
      <div class="chart-placeholder" style="min-height: 280px;">
       Device Online/Offline Status Trend - Time Series Chart
      </div>
     </div>
    </div><!-- Operations Center Page -->
    <div id="operationsPage" class="page-content" style="padding: 32px; display: none;">
     <h1 style="color: white; font-size: 32px; font-weight: bold; margin-bottom: 32px;">Operations Center</h1><!-- Filter Bar -->
     <div style="background: white; border-radius: 12px; padding: 20px; margin-bottom: 24px;"><label style="display: block; margin-bottom: 8px; font-weight: 500; color: #374151;">Filter by Store</label> <select style="width: 100%; max-width: 300px; padding: 10px; border: 1px solid #d1d5db; border-radius: 8px;"> <option>All Stores</option> <option>Store A - Downtown</option> <option>Store B - North District</option> <option>Store C - South District</option> </select>
     </div><!-- Lease Statistics -->
     <h2 style="color: white; font-size: 24px; font-weight: bold; margin-bottom: 20px;">Lease Statistics</h2>
     <div style="display: grid; grid-template-columns: repeat(auto-fit, minmax(200px, 1fr)); gap: 16px; margin-bottom: 32px;">
      <div style="background: white; border-radius: 12px; padding: 20px;">
       <div style="color: #6b7280; font-size: 13px; margin-bottom: 8px;">
        Normal Leasing Orders
       </div>
       <div style="font-size: 24px; font-weight: bold; color: #10b981;">
        845
       </div>
      </div>
      <div style="background: white; border-radius: 12px; padding: 20px;">
       <div style="color: #6b7280; font-size: 13px; margin-bottom: 8px;">
        Defaulted Orders
       </div>
       <div style="font-size: 24px; font-weight: bold; color: #ef4444;">
        23
       </div>
      </div>
      <div style="background: white; border-radius: 12px; padding: 20px;">
       <div style="color: #6b7280; font-size: 13px; margin-bottom: 8px;">
        Expired Orders
       </div>
       <div style="font-size: 24px; font-weight: bold; color: #f59e0b;">
        12
       </div>
      </div>
      <div style="background: white; border-radius: 12px; padding: 20px;">
       <div style="color: #6b7280; font-size: 13px; margin-bottom: 8px;">
        Today's Received Rent
       </div>
       <div style="font-size: 24px; font-weight: bold; color: #3b82f6;">
        RM 12,450
       </div>
      </div>
      <div style="background: white; border-radius: 12px; padding: 20px;">
       <div style="color: #6b7280; font-size: 13px; margin-bottom: 8px;">
        Retention Lease Deposit
       </div>
       <div style="font-size: 24px; font-weight: bold; color: #8b5cf6;">
        RM 45,890
       </div>
      </div>
     </div><!-- Battery Swap Statistics -->
     <h2 style="color: white; font-size: 24px; font-weight: bold; margin-bottom: 20px;">Battery Swap Statistics</h2>
     <div style="display: grid; grid-template-columns: repeat(auto-fit, minmax(200px, 1fr)); gap: 16px; margin-bottom: 32px;">
      <div style="background: white; border-radius: 12px; padding: 20px;">
       <div style="color: #6b7280; font-size: 13px; margin-bottom: 8px;">
        Revenue Amount
       </div>
       <div style="font-size: 24px; font-weight: bold; color: #10b981;">
        RM 89,450
       </div>
      </div>
      <div style="background: white; border-radius: 12px; padding: 20px;">
       <div style="color: #6b7280; font-size: 13px; margin-bottom: 8px;">
        Swapped Energy
       </div>
       <div style="font-size: 24px; font-weight: bold; color: #f59e0b;">
        3,847 kWh
       </div>
      </div>
      <div style="background: white; border-radius: 12px; padding: 20px;">
       <div style="color: #6b7280; font-size: 13px; margin-bottom: 8px;">
        Battery Swaps
       </div>
       <div style="font-size: 24px; font-weight: bold; color: #3b82f6;">
        1,245
       </div>
      </div>
      <div style="background: white; border-radius: 12px; padding: 20px;">
       <div style="color: #6b7280; font-size: 13px; margin-bottom: 8px;">
        Active Users
       </div>
       <div style="font-size: 24px; font-weight: bold; color: #8b5cf6;">
        847
       </div>
      </div>
     </div><!-- Station Ranking -->
     <div style="background: white; border-radius: 12px; padding: 24px;">
      <h3 style="font-size: 18px; font-weight: 600; margin-bottom: 16px; color: #1f2937;">Top Stations by Revenue</h3>
      <div style="overflow-x: auto;">
       <table style="width: 100%; border-collapse: collapse;">
        <thead>
         <tr style="background: #f9fafb; border-bottom: 2px solid #e5e7eb;">
          <th style="padding: 12px; text-align: left; font-weight: 600; color: #6b7280;">Rank</th>
          <th style="padding: 12px; text-align: left; font-weight: 600; color: #6b7280;">Station Name</th>
          <th style="padding: 12px; text-align: left; font-weight: 600; color: #6b7280;">Revenue</th>
          <th style="padding: 12px; text-align: left; font-weight: 600; color: #6b7280;">Swaps</th>
         </tr>
        </thead>
        <tbody>
         <tr style="border-bottom: 1px solid #e5e7eb;">
          <td style="padding: 12px; color: #1f2937;">🥇 1</td>
          <td style="padding: 12px; color: #1f2937; font-weight: 500;">Station Alpha</td>
          <td style="padding: 12px; color: #10b981; font-weight: 600;">RM 24,567</td>
          <td style="padding: 12px; color: #6b7280;">342</td>
         </tr>
         <tr style="border-bottom: 1px solid #e5e7eb;">
          <td style="padding: 12px; color: #1f2937;">🥈 2</td>
          <td style="padding: 12px; color: #1f2937; font-weight: 500;">Station Beta</td>
          <td style="padding: 12px; color: #10b981; font-weight: 600;">RM 18,943</td>
          <td style="padding: 12px; color: #6b7280;">267</td>
         </tr>
         <tr>
          <td style="padding: 12px; color: #1f2937;">🥉 3</td>
          <td style="padding: 12px; color: #1f2937; font-weight: 500;">Station Gamma</td>
          <td style="padding: 12px; color: #10b981; font-weight: 600;">RM 15,234</td>
          <td style="padding: 12px; color: #6b7280;">198</td>
         </tr>
        </tbody>
       </table>
      </div>
     </div>
    </div><!-- Battery Swap Events Page -->
    <div id="swapPage" class="page-content" style="padding: 32px; display: none;">
     <h1 style="color: white; font-size: 32px; font-weight: bold; margin-bottom: 32px;">Battery Swap Events</h1><!-- Search Filters -->
     <div style="background: white; border-radius: 12px; padding: 24px; margin-bottom: 24px;">
      <h3 style="font-size: 18px; font-weight: 600; margin-bottom: 16px; color: #1f2937;">Search Filters</h3>
      <div style="display: grid; grid-template-columns: repeat(auto-fit, minmax(250px, 1fr)); gap: 16px;"><input type="text" placeholder="Cabinet Number" style="padding: 10px; border: 1px solid #d1d5db; border-radius: 8px;"> <input type="text" placeholder="Battery Number" style="padding: 10px; border: 1px solid #d1d5db; border-radius: 8px;"> <input type="text" placeholder="User Information" style="padding: 10px; border: 1px solid #d1d5db; border-radius: 8px;"> <button style="padding: 10px 20px; background: linear-gradient(135deg, #667eea 0%, #764ba2 100%); color: white; border: none; border-radius: 8px; font-weight: 600; cursor: pointer;"> <i class="fas fa-search"></i> Search </button>
      </div>
     </div><!-- Event Records -->
     <div style="background: white; border-radius: 12px; padding: 24px;">
      <h3 style="font-size: 18px; font-weight: 600; margin-bottom: 16px; color: #1f2937;">Recent Swap Events</h3>
      <div style="overflow-x: auto;">
       <table style="width: 100%; border-collapse: collapse; font-size: 14px;">
        <thead>
         <tr style="background: #f9fafb; border-bottom: 2px solid #e5e7eb;">
          <th style="padding: 12px; text-align: left; font-weight: 600; color: #6b7280;">Date/Time</th>
          <th style="padding: 12px; text-align: left; font-weight: 600; color: #6b7280;">User</th>
          <th style="padding: 12px; text-align: left; font-weight: 600; color: #6b7280;">Station</th>
          <th style="padding: 12px; text-align: left; font-weight: 600; color: #6b7280;">Battery In</th>
          <th style="padding: 12px; text-align: left; font-weight: 600; color: #6b7280;">Battery Out</th>
          <th style="padding: 12px; text-align: left; font-weight: 600; color: #6b7280;">Status</th>
         </tr>
        </thead>
        <tbody>
         <tr style="border-bottom: 1px solid #e5e7eb;">
          <td style="padding: 12px; color: #1f2937;">2024-01-15 14:32</td>
          <td style="padding: 12px; color: #1f2937;">John Doe</td>
          <td style="padding: 12px; color: #1f2937;">Station Alpha</td>
          <td style="padding: 12px; color: #1f2937;">BAT-2347 (94%)</td>
          <td style="padding: 12px; color: #1f2937;">BAT-1245 (18%)</td>
          <td style="padding: 12px;"><span style="background: #d1fae5; color: #065f46; padding: 4px 12px; border-radius: 12px; font-size: 12px; font-weight: 500;">Completed</span></td>
         </tr>
         <tr style="border-bottom: 1px solid #e5e7eb;">
          <td style="padding: 12px; color: #1f2937;">2024-01-15 14:28</td>
          <td style="padding: 12px; color: #1f2937;">Jane Smith</td>
          <td style="padding: 12px; color: #1f2937;">Station Beta</td>
          <td style="padding: 12px; color: #1f2937;">BAT-8912 (89%)</td>
          <td style="padding: 12px; color: #1f2937;">BAT-4567 (22%)</td>
          <td style="padding: 12px;"><span style="background: #d1fae5; color: #065f46; padding: 4px 12px; border-radius: 12px; font-size: 12px; font-weight: 500;">Completed</span></td>
         </tr>
         <tr>
          <td style="padding: 12px; color: #1f2937;">2024-01-15 14:15</td>
          <td style="padding: 12px; color: #1f2937;">Mike Wilson</td>
          <td style="padding: 12px; color: #1f2937;">Station Gamma</td>
          <td style="padding: 12px; color: #1f2937;">BAT-3421 (92%)</td>
          <td style="padding: 12px; color: #1f2937;">BAT-9087 (15%)</td>
          <td style="padding: 12px;"><span style="background: #fef3c7; color: #92400e; padding: 4px 12px; border-radius: 12px; font-size: 12px; font-weight: 500;">Processing</span></td>
         </tr>
        </tbody>
       </table>
      </div>
     </div>
    </div><!-- Data Center Page -->
    <div id="dataPage" class="page-content" style="padding: 32px; display: none;">
     <h1 style="color: white; font-size: 32px; font-weight: bold; margin-bottom: 32px;">Data Center - Live Map</h1><!-- Top Metrics -->
     <div style="display: grid; grid-template-columns: repeat(auto-fit, minmax(200px, 1fr)); gap: 16px; margin-bottom: 24px;">
      <div style="background: white; border-radius: 12px; padding: 20px;">
       <div style="color: #6b7280; font-size: 13px; margin-bottom: 8px;">
        Total Active Devices
       </div>
       <div style="font-size: 24px; font-weight: bold; color: #1f2937;">
        3,248
       </div>
      </div>
      <div style="background: white; border-radius: 12px; padding: 20px;">
       <div style="color: #6b7280; font-size: 13px; margin-bottom: 8px;">
        Total Running Time
       </div>
       <div style="font-size: 24px; font-weight: bold; color: #1f2937;">
        24,567 hrs
       </div>
      </div>
      <div style="background: white; border-radius: 12px; padding: 20px;">
       <div style="color: #6b7280; font-size: 13px; margin-bottom: 8px;">
        Online Rate
       </div>
       <div style="font-size: 24px; font-weight: bold; color: #10b981;">
        94.3%
       </div>
      </div>
     </div><!-- Map Container -->
     <div style="background: white; border-radius: 12px; padding: 24px; margin-bottom: 24px;">
      <div style="display: flex; justify-content: space-between; align-items: center; margin-bottom: 16px;">
       <h3 style="font-size: 18px; font-weight: 600; color: #1f2937;">Live Station Map</h3>
       <div style="display: flex; gap: 12px;"><button style="padding: 8px 16px; background: #f3f4f6; border: none; border-radius: 8px; font-size: 13px; cursor: pointer;"> <i class="fas fa-charging-station"></i> Stations </button> <button style="padding: 8px 16px; background: #f3f4f6; border: none; border-radius: 8px; font-size: 13px; cursor: pointer;"> <i class="fas fa-battery-full"></i> Batteries </button> <button style="padding: 8px 16px; background: #f3f4f6; border: none; border-radius: 8px; font-size: 13px; cursor: pointer;"> <i class="fas fa-motorcycle"></i> Vehicles </button>
       </div>
      </div>
      <div id="mapContainer" style="background: #f0f4f8; border-radius: 8px; height: 500px; position: relative; overflow: hidden;"><!-- Map Grid Background -->
       <svg style="position: absolute; width: 100%; height: 100%; opacity: 0.1;"><defs>
         <pattern id="grid" width="40" height="40" patternunits="userSpaceOnUse">
          <path d="M 40 0 L 0 0 0 40" fill="none" stroke="#667eea" stroke-width="1" />
         </pattern>
        </defs> <rect width="100%" height="100%" fill="url(#grid)" />
       </svg><!-- Station Markers -->
       <div id="stationMarkers" style="position: relative; width: 100%; height: 100%;"><!-- Downtown Stations -->
        <div class="station-marker" data-station="Alpha" data-status="online" style="position: absolute; left: 25%; top: 30%;">
         <div style="width: 50px; height: 50px; background: #10b981; border-radius: 50%; display: flex; align-items: center; justify-content: center; color: white; font-weight: bold; box-shadow: 0 4px 12px rgba(16, 185, 129, 0.4); cursor: pointer; transition: all 0.3s;" onmouseover="this.style.transform='scale(1.2)'" onmouseout="this.style.transform='scale(1)'"><i class="fas fa-charging-station"></i>
         </div>
         <div style="position: absolute; top: 55px; left: 50%; transform: translateX(-50%); background: white; padding: 8px 12px; border-radius: 6px; box-shadow: 0 2px 8px rgba(0,0,0,0.15); white-space: nowrap; font-size: 12px; font-weight: 600;">
          Alpha <span style="color: #10b981;">● Online</span>
         </div>
        </div>
        <div class="station-marker" data-station="Beta" data-status="online" style="position: absolute; left: 60%; top: 25%;">
         <div style="width: 50px; height: 50px; background: #10b981; border-radius: 50%; display: flex; align-items: center; justify-content: center; color: white; font-weight: bold; box-shadow: 0 4px 12px rgba(16, 185, 129, 0.4); cursor: pointer; transition: all 0.3s;" onmouseover="this.style.transform='scale(1.2)'" onmouseout="this.style.transform='scale(1)'"><i class="fas fa-charging-station"></i>
         </div>
         <div style="position: absolute; top: 55px; left: 50%; transform: translateX(-50%); background: white; padding: 8px 12px; border-radius: 6px; box-shadow: 0 2px 8px rgba(0,0,0,0.15); white-space: nowrap; font-size: 12px; font-weight: 600;">
          Beta <span style="color: #10b981;">● Online</span>
         </div>
        </div>
        <div class="station-marker" data-station="Gamma" data-status="online" style="position: absolute; left: 45%; top: 55%;">
         <div style="width: 50px; height: 50px; background: #10b981; border-radius: 50%; display: flex; align-items: center; justify-content: center; color: white; font-weight: bold; box-shadow: 0 4px 12px rgba(16, 185, 129, 0.4); cursor: pointer; transition: all 0.3s;" onmouseover="this.style.transform='scale(1.2)'" onmouseout="this.style.transform='scale(1)'"><i class="fas fa-charging-station"></i>
         </div>
         <div style="position: absolute; top: 55px; left: 50%; transform: translateX(-50%); background: white; padding: 8px 12px; border-radius: 6px; box-shadow: 0 2px 8px rgba(0,0,0,0.15); white-space: nowrap; font-size: 12px; font-weight: 600;">
          Gamma <span style="color: #10b981;">● Online</span>
         </div>
        </div>
        <div class="station-marker" data-station="Delta" data-status="warning" style="position: absolute; left: 75%; top: 60%;">
         <div style="width: 50px; height: 50px; background: #f59e0b; border-radius: 50%; display: flex; align-items: center; justify-content: center; color: white; font-weight: bold; box-shadow: 0 4px 12px rgba(245, 158, 11, 0.4); cursor: pointer; transition: all 0.3s; animation: pulse-warning 2s infinite;" onmouseover="this.style.transform='scale(1.2)'" onmouseout="this.style.transform='scale(1)'"><i class="fas fa-charging-station"></i>
         </div>
         <div style="position: absolute; top: 55px; left: 50%; transform: translateX(-50%); background: white; padding: 8px 12px; border-radius: 6px; box-shadow: 0 2px 8px rgba(0,0,0,0.15); white-space: nowrap; font-size: 12px; font-weight: 600;">
          Delta <span style="color: #f59e0b;">● Low Battery</span>
         </div>
        </div>
        <div class="station-marker" data-station="Epsilon" data-status="offline" style="position: absolute; left: 15%; top: 70%;">
         <div style="width: 50px; height: 50px; background: #ef4444; border-radius: 50%; display: flex; align-items: center; justify-content: center; color: white; font-weight: bold; box-shadow: 0 4px 12px rgba(239, 68, 68, 0.4); cursor: pointer; transition: all 0.3s;" onmouseover="this.style.transform='scale(1.2)'" onmouseout="this.style.transform='scale(1)'"><i class="fas fa-charging-station"></i>
         </div>
         <div style="position: absolute; top: 55px; left: 50%; transform: translateX(-50%); background: white; padding: 8px 12px; border-radius: 6px; box-shadow: 0 2px 8px rgba(0,0,0,0.15); white-space: nowrap; font-size: 12px; font-weight: 600;">
          Epsilon <span style="color: #ef4444;">● Offline</span>
         </div>
        </div><!-- Active Vehicles -->
        <div class="vehicle-marker" style="position: absolute; left: 35%; top: 40%; animation: vehicle-move-1 10s infinite;">
         <div style="width: 30px; height: 30px; background: #3b82f6; border-radius: 50%; display: flex; align-items: center; justify-content: center; color: white; font-size: 14px; box-shadow: 0 2px 8px rgba(59, 130, 246, 0.5);"><i class="fas fa-motorcycle"></i>
         </div>
        </div>
        <div class="vehicle-marker" style="position: absolute; left: 55%; top: 35%; animation: vehicle-move-2 12s infinite;">
         <div style="width: 30px; height: 30px; background: #8b5cf6; border-radius: 50%; display: flex; align-items: center; justify-content: center; color: white; font-size: 14px; box-shadow: 0 2px 8px rgba(139, 92, 246, 0.5);"><i class="fas fa-motorcycle"></i>
         </div>
        </div>
        <div class="vehicle-marker" style="position: absolute; left: 40%; top: 65%; animation: vehicle-move-3 15s infinite;">
         <div style="width: 30px; height: 30px; background: #06b6d4; border-radius: 50%; display: flex; align-items: center; justify-content: center; color: white; font-size: 14px; box-shadow: 0 2px 8px rgba(6, 182, 212, 0.5);"><i class="fas fa-motorcycle"></i>
         </div>
        </div>
       </div>
      </div>
      <style>
            @keyframes pulse-warning {
              0%, 100% { opacity: 1; }
              50% { opacity: 0.6; }
            }
            
            @keyframes vehicle-move-1 {
              0%, 100% { transform: translate(0, 0); }
              50% { transform: translate(100px, 50px); }
            }
            
            @keyframes vehicle-move-2 {
              0%, 100% { transform: translate(0, 0); }
              50% { transform: translate(-80px, 80px); }
            }
            
            @keyframes vehicle-move-3 {
              0%, 100% { transform: translate(0, 0); }
              50% { transform: translate(120px, -60px); }
            }
          </style>
     </div><!-- Bike Status List -->
     <div style="background: white; border-radius: 12px; padding: 24px;">
      <h3 style="font-size: 18px; font-weight: 600; margin-bottom: 16px; color: #1f2937;">Active Bikes Status</h3>
      <div style="display: grid; gap: 16px;">
       <div style="border: 1px solid #e5e7eb; border-radius: 8px; padding: 16px;">
        <div style="display: flex; justify-content: space-between; align-items: start; margin-bottom: 12px;">
         <div>
          <div style="font-weight: 600; color: #1f2937; margin-bottom: 4px;">
           Vehicle ID: EV-2847
          </div>
          <div style="font-size: 13px; color: #6b7280;">
           Model: Thunder X-Pro
          </div>
         </div><span style="background: #d1fae5; color: #065f46; padding: 4px 12px; border-radius: 12px; font-size: 12px; font-weight: 500;">Active</span>
        </div>
        <div style="display: grid; grid-template-columns: repeat(auto-fit, minmax(120px, 1fr)); gap: 12px; font-size: 13px;">
         <div>
          <div style="color: #6b7280;">
           Battery
          </div>
          <div style="font-weight: 600; color: #10b981;">
           87%
          </div>
         </div>
         <div>
          <div style="color: #6b7280;">
           Mileage
          </div>
          <div style="font-weight: 600; color: #1f2937;">
           1,245 km
          </div>
         </div>
         <div>
          <div style="color: #6b7280;">
           Location
          </div>
          <div style="font-weight: 600; color: #1f2937;">
           Downtown
          </div>
         </div>
         <div>
          <div style="color: #6b7280;">
           Last Update
          </div>
          <div style="font-weight: 600; color: #1f2937;">
           2 min ago
          </div>
         </div>
        </div>
       </div>
       <div style="border: 1px solid #e5e7eb; border-radius: 8px; padding: 16px;">
        <div style="display: flex; justify-content: space-between; align-items: start; margin-bottom: 12px;">
         <div>
          <div style="font-weight: 600; color: #1f2937; margin-bottom: 4px;">
           Vehicle ID: EV-1923
          </div>
          <div style="font-size: 13px; color: #6b7280;">
           Model: Swift E-200
          </div>
         </div><span style="background: #fef3c7; color: #92400e; padding: 4px 12px; border-radius: 12px; font-size: 12px; font-weight: 500;">Charging</span>
        </div>
        <div style="display: grid; grid-template-columns: repeat(auto-fit, minmax(120px, 1fr)); gap: 12px; font-size: 13px;">
         <div>
          <div style="color: #6b7280;">
           Battery
          </div>
          <div style="font-weight: 600; color: #f59e0b;">
           34%
          </div>
         </div>
         <div>
          <div style="color: #6b7280;">
           Mileage
          </div>
          <div style="font-weight: 600; color: #1f2937;">
           2,187 km
          </div>
         </div>
         <div>
          <div style="color: #6b7280;">
           Location
          </div>
          <div style="font-weight: 600; color: #1f2937;">
           Station Beta
          </div>
         </div>
         <div>
          <div style="color: #6b7280;">
           Last Update
          </div>
          <div style="font-weight: 600; color: #1f2937;">
           5 min ago
          </div>
         </div>
        </div>
       </div>
      </div>
     </div>
    </div><!-- Vehicle Statistics Page -->
    <div id="vehiclesPage" class="page-content" style="padding: 32px; display: none;">
     <h1 style="color: white; font-size: 32px; font-weight: bold; margin-bottom: 32px;">Vehicle Statistics</h1><!-- Vehicle Metrics -->
     <div style="display: grid; grid-template-columns: repeat(auto-fit, minmax(200px, 1fr)); gap: 16px; margin-bottom: 32px;">
      <div style="background: white; border-radius: 12px; padding: 20px;">
       <div style="color: #6b7280; font-size: 13px; margin-bottom: 8px;">
        Riding Mileage
       </div>
       <div style="font-size: 24px; font-weight: bold; color: #1f2937;">
        487,234 km
       </div>
      </div>
      <div style="background: white; border-radius: 12px; padding: 20px;">
       <div style="color: #6b7280; font-size: 13px; margin-bottom: 8px;">
        Avg Riding Time
       </div>
       <div style="font-size: 24px; font-weight: bold; color: #1f2937;">
        2.4 hrs
       </div>
      </div>
      <div style="background: white; border-radius: 12px; padding: 20px;">
       <div style="color: #6b7280; font-size: 13px; margin-bottom: 8px;">
        Total Energy Consumption
       </div>
       <div style="font-size: 24px; font-weight: bold; color: #1f2937;">
        12,847 kWh
       </div>
      </div>
      <div style="background: white; border-radius: 12px; padding: 20px;">
       <div style="color: #6b7280; font-size: 13px; margin-bottom: 8px;">
        Swap Times
       </div>
       <div style="font-size: 24px; font-weight: bold; color: #1f2937;">
        3,458
       </div>
      </div>
      <div style="background: white; border-radius: 12px; padding: 20px;">
       <div style="color: #6b7280; font-size: 13px; margin-bottom: 8px;">
        Active Vehicles
       </div>
       <div style="font-size: 24px; font-weight: bold; color: #10b981;">
        1,847
       </div>
      </div>
     </div><!-- Charts -->
     <div style="display: grid; grid-template-columns: repeat(auto-fit, minmax(400px, 1fr)); gap: 24px; margin-bottom: 32px;">
      <div style="background: white; border-radius: 12px; padding: 24px;">
       <h3 style="font-size: 18px; font-weight: 600; margin-bottom: 16px; color: #1f2937;">Riding Mileage Trend</h3>
       <div class="chart-placeholder">
        30-Day Mileage Trend Chart
       </div>
      </div>
      <div style="background: white; border-radius: 12px; padding: 24px;">
       <h3 style="font-size: 18px; font-weight: 600; margin-bottom: 16px; color: #1f2937;">Energy Consumption Trend</h3>
       <div class="chart-placeholder">
        Energy Usage Over Time
       </div>
      </div>
     </div><!-- Rankings -->
     <div style="display: grid; grid-template-columns: repeat(auto-fit, minmax(400px, 1fr)); gap: 24px;">
      <div style="background: white; border-radius: 12px; padding: 24px;">
       <h3 style="font-size: 18px; font-weight: 600; margin-bottom: 16px; color: #1f2937;">Top 10 Models</h3>
       <div style="display: flex; flex-direction: column; gap: 12px;">
        <div style="display: flex; justify-content: space-between; align-items: center; padding: 12px; background: #f9fafb; border-radius: 8px;"><span style="font-weight: 500;">Thunder X-Pro</span> <span style="color: #10b981; font-weight: 600;">487 units</span>
        </div>
        <div style="display: flex; justify-content: space-between; align-items: center; padding: 12px; background: #f9fafb; border-radius: 8px;"><span style="font-weight: 500;">Swift E-200</span> <span style="color: #10b981; font-weight: 600;">342 units</span>
        </div>
        <div style="display: flex; justify-content: space-between; align-items: center; padding: 12px; background: #f9fafb; border-radius: 8px;"><span style="font-weight: 500;">Urban Rider Plus</span> <span style="color: #10b981; font-weight: 600;">298 units</span>
        </div>
       </div>
      </div>
      <div style="background: white; border-radius: 12px; padding: 24px;">
       <h3 style="font-size: 18px; font-weight: 600; margin-bottom: 16px; color: #1f2937;">Top 10 Vehicles by Usage</h3>
       <div style="display: flex; flex-direction: column; gap: 12px;">
        <div style="display: flex; justify-content: space-between; align-items: center; padding: 12px; background: #f9fafb; border-radius: 8px;"><span style="font-weight: 500;">EV-2847</span> <span style="color: #3b82f6; font-weight: 600;">1,847 km</span>
        </div>
        <div style="display: flex; justify-content: space-between; align-items: center; padding: 12px; background: #f9fafb; border-radius: 8px;"><span style="font-weight: 500;">EV-1923</span> <span style="color: #3b82f6; font-weight: 600;">1,723 km</span>
        </div>
        <div style="display: flex; justify-content: space-between; align-items: center; padding: 12px; background: #f9fafb; border-radius: 8px;"><span style="font-weight: 500;">EV-3456</span> <span style="color: #3b82f6; font-weight: 600;">1,654 km</span>
        </div>
       </div>
      </div>
     </div>
    </div><!-- Station Analytics Page -->
    <div id="stationPage" class="page-content" style="padding: 32px; display: none;">
     <h1 style="color: white; font-size: 32px; font-weight: bold; margin-bottom: 32px;">Station Swap Statistics</h1><!-- Station KPIs -->
     <div style="display: grid; grid-template-columns: repeat(auto-fit, minmax(200px, 1fr)); gap: 16px; margin-bottom: 32px;">
      <div style="background: white; border-radius: 12px; padding: 20px;">
       <div style="color: #6b7280; font-size: 13px; margin-bottom: 8px;">
        Successful Swaps
       </div>
       <div style="font-size: 24px; font-weight: bold; color: #10b981;">
        3,245
       </div>
      </div>
      <div style="background: white; border-radius: 12px; padding: 20px;">
       <div style="color: #6b7280; font-size: 13px; margin-bottom: 8px;">
        Failed Swaps
       </div>
       <div style="font-size: 24px; font-weight: bold; color: #ef4444;">
        43
       </div>
      </div>
      <div style="background: white; border-radius: 12px; padding: 20px;">
       <div style="color: #6b7280; font-size: 13px; margin-bottom: 8px;">
        Energy Delivered
       </div>
       <div style="font-size: 24px; font-weight: bold; color: #1f2937;">
        8,945 kWh
       </div>
      </div>
      <div style="background: white; border-radius: 12px; padding: 20px;">
       <div style="color: #6b7280; font-size: 13px; margin-bottom: 8px;">
        Unique Users
       </div>
       <div style="font-size: 24px; font-weight: bold; color: #8b5cf6;">
        1,234
       </div>
      </div>
      <div style="background: white; border-radius: 12px; padding: 20px;">
       <div style="color: #6b7280; font-size: 13px; margin-bottom: 8px;">
        Active Stations
       </div>
       <div style="font-size: 24px; font-weight: bold; color: #3b82f6;">
        142
       </div>
      </div>
     </div><!-- Trend Charts -->
     <div style="display: grid; grid-template-columns: repeat(auto-fit, minmax(400px, 1fr)); gap: 24px; margin-bottom: 32px;">
      <div style="background: white; border-radius: 12px; padding: 24px;">
       <h3 style="font-size: 18px; font-weight: 600; margin-bottom: 16px; color: #1f2937;">Swap Volume Trend</h3>
       <div class="chart-placeholder">
        Daily Swap Volume Chart
       </div>
      </div>
      <div style="background: white; border-radius: 12px; padding: 24px;">
       <h3 style="font-size: 18px; font-weight: 600; margin-bottom: 16px; color: #1f2937;">Energy Consumption Trend</h3>
       <div class="chart-placeholder">
        Energy Delivery Over Time
       </div>
      </div>
     </div><!-- Station Performance Table -->
     <div style="background: white; border-radius: 12px; padding: 24px;">
      <h3 style="font-size: 18px; font-weight: 600; margin-bottom: 16px; color: #1f2937;">Station Performance Ranking</h3>
      <div style="overflow-x: auto;">
       <table style="width: 100%; border-collapse: collapse;">
        <thead>
         <tr style="background: #f9fafb; border-bottom: 2px solid #e5e7eb;">
          <th style="padding: 12px; text-align: left; font-weight: 600; color: #6b7280;">Rank</th>
          <th style="padding: 12px; text-align: left; font-weight: 600; color: #6b7280;">Station Name</th>
          <th style="padding: 12px; text-align: left; font-weight: 600; color: #6b7280;">Total Swaps</th>
          <th style="padding: 12px; text-align: left; font-weight: 600; color: #6b7280;">Energy (kWh)</th>
          <th style="padding: 12px; text-align: left; font-weight: 600; color: #6b7280;">Success Rate</th>
         </tr>
        </thead>
        <tbody>
         <tr style="border-bottom: 1px solid #e5e7eb;">
          <td style="padding: 12px; color: #1f2937;">🥇 1</td>
          <td style="padding: 12px; color: #1f2937; font-weight: 500;">Station Alpha</td>
          <td style="padding: 12px; color: #1f2937;">542</td>
          <td style="padding: 12px; color: #1f2937;">1,847 kWh</td>
          <td style="padding: 12px; color: #10b981; font-weight: 600;">98.5%</td>
         </tr>
         <tr style="border-bottom: 1px solid #e5e7eb;">
          <td style="padding: 12px; color: #1f2937;">🥈 2</td>
          <td style="padding: 12px; color: #1f2937; font-weight: 500;">Station Beta</td>
          <td style="padding: 12px; color: #1f2937;">487</td>
          <td style="padding: 12px; color: #1f2937;">1,623 kWh</td>
          <td style="padding: 12px; color: #10b981; font-weight: 600;">97.8%</td>
         </tr>
         <tr style="border-bottom: 1px solid #e5e7eb;">
          <td style="padding: 12px; color: #1f2937;">🥉 3</td>
          <td style="padding: 12px; color: #1f2937; font-weight: 500;">Station Gamma</td>
          <td style="padding: 12px; color: #1f2937;">423</td>
          <td style="padding: 12px; color: #1f2937;">1,456 kWh</td>
          <td style="padding: 12px; color: #10b981; font-weight: 600;">96.2%</td>
         </tr>
         <tr>
          <td style="padding: 12px; color: #1f2937;">4</td>
          <td style="padding: 12px; color: #1f2937; font-weight: 500;">Station Delta</td>
          <td style="padding: 12px; color: #1f2937;">389</td>
          <td style="padding: 12px; color: #1f2937;">1,298 kWh</td>
          <td style="padding: 12px; color: #10b981; font-weight: 600;">95.7%</td>
         </tr>
        </tbody>
       </table>
      </div>
     </div>
    </div>
   </div>
  </div>
  <script>
    const defaultConfig = {
      platform_name: "EV Bike Management",
      company_name: "Smart Mobility Solutions",
      background_color: "#1e3a8a",
      card_background: "#ffffff",
      primary_action: "#667eea",
      text_color: "#1f2937",
      accent_color: "#10b981"
    };

    let config = window.elementSdk ? window.elementSdk.config : { ...defaultConfig };

    async function onConfigChange(newConfig) {
      const platformNameEl = document.getElementById('platformName');
      const companyNameEl = document.getElementById('companyName');
      
      if (platformNameEl) {
        platformNameEl.textContent = newConfig.platform_name || defaultConfig.platform_name;
      }
      if (companyNameEl) {
        companyNameEl.textContent = newConfig.company_name || defaultConfig.company_name;
      }

      const appEl = document.getElementById('app');
      if (appEl) {
        appEl.style.background = `linear-gradient(135deg, ${newConfig.background_color || defaultConfig.background_color} 0%, ${newConfig.primary_action || defaultConfig.primary_action} 100%)`;
      }
    }

    function mapToCapabilities(config) {
      return {
        recolorables: [
          {
            get: () => config.background_color || defaultConfig.background_color,
            set: (value) => {
              config.background_color = value;
              window.elementSdk.setConfig({ background_color: value });
            }
          },
          {
            get: () => config.card_background || defaultConfig.card_background,
            set: (value) => {
              config.card_background = value;
              window.elementSdk.setConfig({ card_background: value });
            }
          },
          {
            get: () => config.primary_action || defaultConfig.primary_action,
            set: (value) => {
              config.primary_action = value;
              window.elementSdk.setConfig({ primary_action: value });
            }
          },
          {
            get: () => config.text_color || defaultConfig.text_color,
            set: (value) => {
              config.text_color = value;
              window.elementSdk.setConfig({ text_color: value });
            }
          },
          {
            get: () => config.accent_color || defaultConfig.accent_color,
            set: (value) => {
              config.accent_color = value;
              window.elementSdk.setConfig({ accent_color: value });
            }
          }
        ],
        borderables: [],
        fontEditable: undefined,
        fontSizeable: undefined
      };
    }

    function mapToEditPanelValues(config) {
      return new Map([
        ["platform_name", config.platform_name || defaultConfig.platform_name],
        ["company_name", config.company_name || defaultConfig.company_name]
      ]);
    }

    if (window.elementSdk) {
      window.elementSdk.init({
        defaultConfig,
        onConfigChange,
        mapToCapabilities,
        mapToEditPanelValues
      });
    }

    // Make navigation function globally available
    window.navigateToPage = function(pageName) {
      // Hide all pages
      document.getElementById('homePage').style.display = 'none';
      document.getElementById('assetPage').style.display = 'none';
      document.getElementById('operationsPage').style.display = 'none';
      document.getElementById('swapPage').style.display = 'none';
      document.getElementById('dataPage').style.display = 'none';
      document.getElementById('vehiclesPage').style.display = 'none';
      document.getElementById('stationPage').style.display = 'none';
      
      // Show target page
      const targetPage = document.getElementById(pageName + 'Page');
      if (targetPage) {
        targetPage.style.display = 'block';
      }
      
      // Update sidebar active state
      const allSidebarItems = document.querySelectorAll('.sidebar-item');
      allSidebarItems.forEach(item => {
        item.classList.remove('active');
        item.style.color = 'rgba(255, 255, 255, 0.8)';
      });
      
      const activeSidebarItem = document.querySelector('.sidebar-item[data-page="' + pageName + '"]');
      if (activeSidebarItem) {
        activeSidebarItem.classList.add('active');
        activeSidebarItem.style.color = 'white';
      }
    };

    // Wait for DOM to be ready
    document.addEventListener('DOMContentLoaded', function() {
      // Navigation function
      function navigateToPage(pageName) {
        console.log('Navigating to:', pageName);
        
        // Hide all pages
        document.getElementById('homePage').style.display = 'none';
        document.getElementById('assetPage').style.display = 'none';
        document.getElementById('operationsPage').style.display = 'none';
        document.getElementById('swapPage').style.display = 'none';
        document.getElementById('dataPage').style.display = 'none';
        document.getElementById('vehiclesPage').style.display = 'none';
        document.getElementById('stationPage').style.display = 'none';
        
        // Show target page
        const targetPage = document.getElementById(pageName + 'Page');
        if (targetPage) {
          targetPage.style.display = 'block';
          console.log('Showing page:', pageName);
        }
        
        // Update sidebar active state
        const allSidebarItems = document.querySelectorAll('.sidebar-item');
        allSidebarItems.forEach(item => {
          item.classList.remove('active');
          item.style.color = 'rgba(255, 255, 255, 0.8)';
        });
        
        const activeSidebarItem = document.querySelector(`.sidebar-item[data-page="${pageName}"]`);
        if (activeSidebarItem) {
          activeSidebarItem.classList.add('active');
          activeSidebarItem.style.color = 'white';
        }
      }

      // Sidebar Navigation with direct onclick
      const sidebarItems = document.querySelectorAll('.sidebar-item');
      sidebarItems.forEach(item => {
        item.onclick = function(e) {
          e.stopPropagation();
          const pageName = this.getAttribute('data-page');
          console.log('Sidebar clicked:', pageName);
          navigateToPage(pageName);
          return false;
        };
      });

      // Menu Card Navigation
      const menuCards = document.querySelectorAll('.menu-card');
      menuCards.forEach(card => {
        card.onclick = function(e) {
          e.stopPropagation();
          const pageName = this.getAttribute('data-page');
          console.log('Card clicked:', pageName);
          if (pageName) {
            navigateToPage(pageName);
          }
          return false;
        };
      });
    });

    // Battery trend filter function
    window.filterBatteryTrend = function(filterType) {
      // Update button styles
      const allButtons = document.querySelectorAll('.time-filter-btn');
      allButtons.forEach(btn => {
        if (btn.getAttribute('data-filter') === filterType) {
          btn.style.background = '#667eea';
          btn.style.color = 'white';
          btn.classList.add('active');
        } else {
          btn.style.background = '#f3f4f6';
          btn.style.color = '#6b7280';
          btn.classList.remove('active');
        }
      });

      // Show message for custom filter
      if (filterType === 'custom') {
        // Create inline date picker UI
        const chartPlaceholder = document.querySelector('#assetPage .chart-placeholder');
        if (chartPlaceholder) {
          const tempMessage = document.createElement('div');
          tempMessage.style.cssText = 'position: absolute; top: 50%; left: 50%; transform: translate(-50%, -50%); background: white; padding: 16px 24px; border-radius: 8px; box-shadow: 0 4px 12px rgba(0,0,0,0.15); font-size: 14px; color: #374151; z-index: 10;';
          tempMessage.textContent = '📅 Custom date range selector would appear here';
          chartPlaceholder.style.position = 'relative';
          chartPlaceholder.appendChild(tempMessage);
          setTimeout(() => tempMessage.remove(), 2000);
        }
      }
      
      console.log('Filtering battery trend by:', filterType);
    };

    // Show alarm details function
    window.showAlarmDetails = function() {
      // Create notification
      const notification = document.createElement('div');
      notification.style.cssText = 'position: fixed; top: 20px; right: 20px; background: linear-gradient(135deg, #667eea 0%, #764ba2 100%); color: white; padding: 16px 24px; border-radius: 12px; box-shadow: 0 8px 24px rgba(102, 126, 234, 0.4); z-index: 1000; font-weight: 500;';
      notification.innerHTML = '<i class="fas fa-bell"></i> Opening Alarm Management Page...';
      document.body.appendChild(notification);
      setTimeout(() => notification.remove(), 2000);
      
      console.log('Navigating to Alarm Management page');
    };

    onConfigChange(config);
  </script>
 <script>(function(){function c(){var b=a.contentDocument||a.contentWindow.document;if(b){var d=b.createElement('script');d.innerHTML="window.__CF$cv$params={r:'9b2e61ea8741c83e',t:'MTc2NjU2MTMxMi4wMDAwMDA='};var a=document.createElement('script');a.nonce='';a.src='/cdn-cgi/challenge-platform/scripts/jsd/main.js';document.getElementsByTagName('head')[0].appendChild(a);";b.getElementsByTagName('head')[0].appendChild(d)}}if(document.body){var a=document.createElement('iframe');a.height=1;a.width=1;a.style.position='absolute';a.style.top=0;a.style.left=0;a.style.border='none';a.style.visibility='hidden';document.body.appendChild(a);if('loading'!==document.readyState)c();else if(window.addEventListener)document.addEventListener('DOMContentLoaded',c);else{var e=document.onreadystatechange||function(){};document.onreadystatechange=function(b){e(b);'loading'!==document.readyState&&(document.onreadystatechange=e,c())}}}})();</script></body>
</html>
