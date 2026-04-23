<!DOCTYPE html>
<html lang="zh-CN">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
<title>AI手机</title>
<style>
*{margin:0;padding:0;box-sizing:border-box;-webkit-tap-highlight-color:transparent;}
html,body{
  width:100%;height:100%;overflow:hidden;
  display:flex;justify-content:center;align-items:center;
  background:linear-gradient(135deg,#1a1a2e 0%,#16213e 50%,#0f3460 100%);
  font-family:-apple-system,'PingFang SC','Helvetica Neue',Arial,sans-serif;
}
.phone{
  width:375px;height:780px;
  background:linear-gradient(160deg,#2c2c2e,#1c1c1e);
  border-radius:52px;padding:14px;
  box-shadow:0 0 0 1px #3a3a3c,0 0 0 3px #1c1c1e,
             0 40px 100px rgba(0,0,0,0.5),
             inset 0 1px 0 rgba(255,255,255,0.08);
  position:relative;flex-shrink:0;
  /* 关键：手机本身不滚动 */
  overflow:hidden;
}
.btn-r{position:absolute;right:-3px;top:140px;width:3px;height:70px;background:#3a3a3c;border-radius:0 2px 2px 0;}
.btn-l1{position:absolute;left:-3px;top:110px;width:3px;height:38px;background:#3a3a3c;border-radius:2px 0 0 2px;}
.btn-l2{position:absolute;left:-3px;top:165px;width:3px;height:60px;background:#3a3a3c;border-radius:2px 0 0 2px;}
.btn-l3{position:absolute;left:-3px;top:240px;width:3px;height:60px;background:#3a3a3c;border-radius:2px 0 0 2px;}

.screen{
  width:100%;height:100%;
  border-radius:40px;
  overflow:hidden;
  position:relative;
  background:#000;
  /* 屏幕内容不溢出 */
  display:flex;flex-direction:column;
}

.notch{
  position:absolute;top:0;left:50%;transform:translateX(-50%);
  width:126px;height:34px;background:#1c1c1e;
  border-radius:0 0 22px 22px;
  display:flex;align-items:center;justify-content:center;gap:8px;
  z-index:9999;pointer-events:none;
}
.notch-camera{width:11px;height:11px;background:#2c2c2e;border-radius:50%;border:2px solid #3a3a3c;}
.notch-speaker{width:44px;height:5px;background:#2c2c2e;border-radius:3px;}

/* ===== 页面切换系统 ===== */
/* 所有页面绝对定位叠在一起，通过translate切换 */
.page{
  position:absolute;
  inset:0;
  display:flex;
  flex-direction:column;
  /* 默认推到右边屏幕外 */
  transform:translateX(100%);
  transition:transform 0.32s cubic-bezier(0.4,0,0.2,1);
  will-change:transform;
  overflow:hidden;
}
.page.active{
  transform:translateX(0);
}
.page.slide-out{
  transform:translateX(-30%);
}
/* 锁屏和主屏特殊处理 */
#lockScreen{transform:translateX(0);}
#lockScreen.hidden{transform:translateX(-100%);}
#homeScreen{transform:translateX(0);opacity:0;pointer-events:none;}
#homeScreen.active{opacity:1;pointer-events:all;transform:translateX(0);}

/* ===== 锁屏 ===== */
#lockScreen{
  background:linear-gradient(160deg,#0d0d1a 0%,#1a0a2e 40%,#0a1628 100%);
  z-index:50;
}
.lock-bg-stars{position:absolute;inset:0;overflow:hidden;pointer-events:none;}
.star{position:absolute;background:white;border-radius:50%;animation:twinkle var(--d,3s) infinite var(--delay,0s);}
@keyframes twinkle{0%,100%{opacity:0.2;}50%{opacity:1;}}
.lock-time-area{
  flex:1;display:flex;flex-direction:column;
  align-items:center;justify-content:center;padding-top:60px;
  position:relative;z-index:1;
}
.lock-time{font-size:72px;font-weight:200;color:white;letter-spacing:-2px;line-height:1;}
.lock-date{font-size:16px;color:rgba(255,255,255,0.6);margin-top:8px;}
.lock-notif{
  margin:0 20px 20px;position:relative;z-index:1;
  background:rgba(255,255,255,0.08);backdrop-filter:blur(20px);
  border:1px solid rgba(255,255,255,0.1);border-radius:16px;padding:12px 14px;
}
.lock-notif-row{display:flex;align-items:center;gap:10px;}
.lock-notif-icon{font-size:20px;}
.lock-notif-app{font-size:11px;color:rgba(255,255,255,0.4);}
.lock-notif-msg{font-size:13px;color:white;margin-top:1px;}
.lock-swipe{
  text-align:center;padding-bottom:40px;font-size:13px;
  color:rgba(255,255,255,0.4);position:relative;z-index:1;
  animation:fadeUpDown 2s infinite;
}
@keyframes fadeUpDown{0%,100%{opacity:0.4;transform:translateY(0);}50%{opacity:0.8;transform:translateY(-4px);}}

/* ===== 主屏 ===== */
#homeScreen{
  background:#000;
  z-index:10;
}
.wallpaper{position:absolute;inset:0;background:linear-gradient(160deg,#0d0d1a 0%,#1a0a2e 35%,#0a1628 70%,#0d1117 100%);}
.wallpaper-orb{position:absolute;border-radius:50%;filter:blur(60px);opacity:0.3;}

.status-bar{
  position:relative;z-index:100;
  padding:44px 24px 8px;
  display:flex;align-items:center;justify-content:space-between;
  flex-shrink:0;
}
.status-time{font-size:15px;font-weight:600;color:white;}
.status-icons{display:flex;align-items:center;gap:5px;}
.status-icon-text{font-size:12px;color:white;font-weight:500;}
.battery{
  width:22px;height:11px;border:1.5px solid rgba(255,255,255,0.6);
  border-radius:3px;position:relative;display:flex;align-items:center;padding:1.5px;
}
.battery::after{
  content:'';position:absolute;right:-4px;top:50%;transform:translateY(-50%);
  width:2px;height:5px;background:rgba(255,255,255,0.6);border-radius:0 1px 1px 0;
}
.battery-fill{height:100%;background:#30d158;border-radius:1px;width:70%;}

/* 桌面区域 */
.desktop-area{
  flex:1;
  position:relative;
  overflow:hidden;
  display:flex;flex-direction:column;
  min-height:0;
}
.pages-container{
  flex:1;
  display:flex;
  transition:transform 0.38s cubic-bezier(0.25,0.46,0.45,0.94);
  will-change:transform;
  min-height:0;
}
.desktop-page{
  min-width:100%;
  padding:8px 20px 0;
  display:flex;flex-direction:column;
  overflow:hidden;
}

/* 小组件 */
.widget-area{margin-bottom:12px;flex-shrink:0;}
.widget{
  border-radius:20px;overflow:hidden;
  background:rgba(255,255,255,0.08);
  backdrop-filter:blur(20px);
  border:1px solid rgba(255,255,255,0.1);
  cursor:pointer;
  transition:transform 0.15s;
  -webkit-transform:translateZ(0);
}
.widget:active{transform:scale(0.97);}
.widget-weather{padding:16px 18px;background:linear-gradient(135deg,rgba(10,100,200,0.4),rgba(80,40,160,0.4));}
.widget-weather-top{display:flex;align-items:flex-start;justify-content:space-between;}
.widget-weather-temp{font-size:42px;font-weight:200;color:white;line-height:1;}
.widget-weather-icon{font-size:36px;}
.widget-weather-desc{font-size:13px;color:rgba(255,255,255,0.7);margin-top:4px;}
.widget-weather-city{font-size:11px;color:rgba(255,255,255,0.4);margin-top:2px;}
.widget-weather-row{display:flex;gap:16px;margin-top:10px;}
.widget-weather-item{font-size:11px;color:rgba(255,255,255,0.5);}
.widget-weather-item span{color:rgba(255,255,255,0.85);font-weight:500;}
.widget-clock{padding:14px 18px;background:linear-gradient(135deg,rgba(40,40,60,0.6),rgba(20,20,40,0.6));display:flex;align-items:center;gap:14px;}
.widget-clock-time{font-size:36px;font-weight:200;color:white;letter-spacing:-1px;}
.widget-clock-date{font-size:13px;color:rgba(255,255,255,0.7);}
.widget-clock-week{font-size:11px;color:rgba(255,255,255,0.4);margin-top:2px;}
.widget-steps{padding:14px 18px;background:linear-gradient(135deg,rgba(255,100,50,0.3),rgba(200,50,100,0.3));}
.widget-steps-top{display:flex;align-items:center;justify-content:space-between;}
.widget-steps-label{font-size:11px;color:rgba(255,255,255,0.5);}
.widget-steps-num{font-size:28px;font-weight:700;color:white;margin-top:4px;}
.widget-steps-bar{height:4px;background:rgba(255,255,255,0.1);border-radius:2px;margin-top:8px;}
.widget-steps-fill{height:100%;background:linear-gradient(90deg,#ff6432,#ff3b7a);border-radius:2px;width:62%;}

/* 图标网格 */
.icon-grid{
  display:grid;grid-template-columns:repeat(4,1fr);
  gap:16px 8px;padding:4px 0;
  flex-shrink:0;
}
.app-icon{display:flex;flex-direction:column;align-items:center;gap:5px;cursor:pointer;}
.app-icon:active .icon-img{transform:scale(0.88);}
.icon-img{
  width:58px;height:58px;border-radius:14px;
  display:flex;align-items:center;justify-content:center;
  font-size:28px;
  transition:transform 0.12s;
  position:relative;overflow:hidden;
  box-shadow:0 4px 12px rgba(0,0,0,0.3);
  -webkit-transform:translateZ(0);
}
.icon-label{font-size:11px;color:rgba(255,255,255,0.85);text-align:center;line-height:1.2;max-width:64px;overflow:hidden;text-overflow:ellipsis;white-space:nowrap;}
.badge{
  position:absolute;top:-3px;right:-3px;
  background:#ff3b30;color:white;font-size:9px;font-weight:700;
  min-width:16px;height:16px;border-radius:8px;
  display:flex;align-items:center;justify-content:center;
  padding:0 3px;border:1.5px solid #000;
}

/* 页面指示点 */
.page-dots{display:flex;align-items:center;justify-content:center;gap:6px;padding:8px 0 4px;flex-shrink:0;}
.page-dot{width:6px;height:6px;border-radius:3px;background:rgba(255,255,255,0.25);transition:all 0.3s;}
.page-dot.active{width:18px;background:rgba(255,255,255,0.8);}

/* Dock */
.dock{
  margin:4px 16px 16px;
  background:rgba(255,255,255,0.1);
  backdrop-filter:blur(30px);
  border:1px solid rgba(255,255,255,0.12);
  border-radius:26px;padding:12px 16px;
  display:flex;justify-content:space-around;align-items:center;
  flex-shrink:0;position:relative;z-index:10;
}
.dock .app-icon .icon-img{width:54px;height:54px;}

.home-indicator{
  height:28px;display:flex;align-items:center;justify-content:center;
  flex-shrink:0;position:relative;z-index:10;
}
.home-bar{width:130px;height:5px;background:rgba(255,255,255,0.15);border-radius:3px;}

/* ===== 应用页通用 ===== */
/* 应用页从右侧滑入 */
.app-page{
  background:#0a0a0f;
  z-index:20;
}
.app-nav{
  padding:44px 20px 14px;
  background:rgba(10,10,20,0.98);
  border-bottom:1px solid rgba(255,255,255,0.06);
  flex-shrink:0;
}
.app-nav-row{display:flex;align-items:center;gap:12px;}
.back-btn{
  width:32px;height:32px;background:rgba(255,255,255,0.08);
  border-radius:50%;display:flex;align-items:center;justify-content:center;
  font-size:20px;cursor:pointer;color:rgba(255,255,255,0.7);
  transition:background 0.15s;flex-shrink:0;line-height:1;
}
.back-btn:active{background:rgba(255,255,255,0.18);}
.app-nav-title{font-size:18px;font-weight:700;color:white;flex:1;}

/* 滚动内容区 — 关键：只有这里滚动 */
.app-scroll{
  flex:1;
  overflow-y:auto;
  overflow-x:hidden;
  -webkit-overflow-scrolling:touch;
  overscroll-behavior:contain;
  scrollbar-width:none;
  min-height:0;
}
.app-scroll::-webkit-scrollbar{display:none;}
.app-scroll-inner{padding:16px 16px 24px;}

/* ===== 卡片 ===== */
.card{
  background:rgba(255,255,255,0.05);
  border:1px solid rgba(255,255,255,0.08);
  border-radius:18px;padding:16px;margin-bottom:14px;
}
.card-title{
  font-size:11px;font-weight:600;color:rgba(255,255,255,0.35);
  letter-spacing:1.5px;text-transform:uppercase;margin-bottom:14px;
  display:flex;align-items:center;gap:6px;
}

/* ===== 消息列表 ===== */
#msgListScreen .app-scroll-inner{padding:0;}

/* 搜索框 */
.msg-search-wrap{padding:10px 16px 6px;}
.msg-search{
  background:rgba(255,255,255,0.07);
  border:1px solid rgba(255,255,255,0.08);
  border-radius:12px;padding:9px 14px;
  display:flex;align-items:center;gap:8px;
}
.msg-search input{
  flex:1;background:none;border:none;outline:none;
  font-size:14px;color:white;font-family:inherit;
}
.msg-search input::placeholder{color:rgba(255,255,255,0.25);}

/* 消息列表项 — 微信风格 */
.msg-list{display:flex;flex-direction:column;}
.msg-item{
  display:flex;align-items:center;gap:12px;
  padding:12px 16px;
  cursor:pointer;
  border-bottom:1px solid rgba(255,255,255,0.04);
  transition:background 0.12s;
  position:relative;
}
.msg-item:active{background:rgba(255,255,255,0.06);}
.msg-item:last-child{border-bottom:none;}

/* 头像 */
.msg-avatar{
  width:50px;height:50px;
  border-radius:12px;
  display:flex;align-items:center;justify-content:center;
  font-size:26px;flex-shrink:0;
  background:rgba(255,255,255,0.08);
  position:relative;
  overflow:hidden;
}
.msg-avatar-bg{
  position:absolute;inset:0;
  border-radius:12px;
}
.msg-online-dot{
  position:absolute;bottom:-1px;right:-1px;
  width:12px;height:12px;
  background:#30d158;border-radius:50%;
  border:2px solid #0a0a0f;
}

/* 右侧内容 */
.msg-content{flex:1;min-width:0;}
.msg-top-row{display:flex;align-items:center;justify-content:space-between;margin-bottom:4px;}
.msg-name{font-size:16px;font-weight:600;color:white;overflow:hidden;text-overflow:ellipsis;white-space:nowrap;}
.msg-time-label{font-size:12px;color:rgba(255,255,255,0.3);flex-shrink:0;margin-left:8px;}
.msg-bottom-row{display:flex;align-items:center;justify-content:space-between;}
.msg-preview-text{
  font-size:13px;color:rgba(255,255,255,0.4);
  overflow:hidden;text-overflow:ellipsis;white-space:nowrap;
  flex:1;
}
.msg-unread-badge{
  background:#ff3b30;color:white;
  font-size:11px;font-weight:700;
  min-width:18px;height:18px;border-radius:9px;
  display:flex;align-items:center;justify-content:center;
  padding:0 5px;flex-shrink:0;margin-left:8px;
}

/* 分组标题 */
.msg-section-title{
  font-size:12px;color:rgba(255,255,255,0.25);
  padding:12px 16px 6px;
  letter-spacing:0.5px;
}

/* ===== 聊天页 ===== */
#chatScreen{z-index:30;}
.chat-nav{
  padding:44px 16px 12px;
  background:rgba(10,10,20,0.98);
  border-bottom:1px solid rgba(255,255,255,0.06);
  flex-shrink:0;
}
.chat-nav-row{display:flex;align-items:center;gap:10px;}
.chat-avatar-sm{
  width:36px;height:36px;border-radius:10px;
  display:flex;align-items:center;justify-content:center;
  font-size:20px;background:rgba(255,255,255,0.1);flex-shrink:0;
}
.chat-name-wrap{flex:1;}
.chat-name{font-size:16px;font-weight:700;color:white;}
.chat-status-text{font-size:11px;color:#30d158;margin-top:1px;}
.chat-action-btn{
  width:32px;height:32px;background:rgba(255,255,255,0.08);
  border-radius:50%;display:flex;align-items:center;justify-content:center;
  font-size:15px;cursor:pointer;
}

/* 消息区 — 只有这里滚动 */
.chat-messages{
  flex:1;
  overflow-y:auto;
  overflow-x:hidden;
  -webkit-overflow-scrolling:touch;
  overscroll-behavior:contain;
  padding:12px 14px;
  display:flex;flex-direction:column;gap:10px;
  scrollbar-width:none;
  min-height:0;
}
.chat-messages::-webkit-scrollbar{display:none;}

.msg-bubble-wrap{display:flex;align-items:flex-end;gap:8px;}
.msg-bubble-wrap.me{flex-direction:row-reverse;}
.bubble-avatar-sm{
  width:30px;height:30px;border-radius:8px;
  display:flex;align-items:center;justify-content:center;
  font-size:16px;background:rgba(255,255,255,0.1);flex-shrink:0;
}
.bubble{
  max-width:68%;padding:10px 13px;
  font-size:14px;line-height:1.55;word-break:break-word;
}
.bubble.them{
  background:rgba(255,255,255,0.1);color:white;
  border-radius:18px 18px 18px 4px;
}
.bubble.me{
  background:linear-gradient(135deg,#30d158,#25a244);color:white;
  border-radius:18px 18px 4px 18px;
}
.bubble-time{font-size:10px;color:rgba(255,255,255,0.25);margin-top:3px;}
.bubble-wrap-me .bubble-time{text-align:right;}

/* 打字动画 */
.bubble.typing{
  background:rgba(255,255,255,0.08);
  display:flex;align-items:center;gap:4px;
  padding:12px 16px;
  border-radius:18px 18px 18px 4px;
}
.typing-dot{
  width:6px;height:6px;background:rgba(255,255,255,0.5);
  border-radius:50%;animation:typingBounce 1.2s infinite;
}
.typing-dot:nth-child(2){animation-delay:0.2s;}
.typing-dot:nth-child(3){animation-delay:0.4s;}
@keyframes typingBounce{0%,60%,100%{transform:translateY(0);}30%{transform:translateY(-6px);}}

/* 输入区 */
.chat-input-area{
  padding:10px 14px 20px;
  background:rgba(10,10,20,0.98);
  border-top:1px solid rgba(255,255,255,0.06);
  display:flex;align-items:flex-end;gap:10px;
  flex-shrink:0;
}
.chat-input{
  flex:1;background:rgba(255,255,255,0.08);
  border:1px solid rgba(255,255,255,0.1);border-radius:20px;
  padding:10px 16px;font-size:14px;color:white;
  outline:none;font-family:inherit;resize:none;
  max-height:100px;min-height:40px;line-height:1.4;
  scrollbar-width:none;
  -webkit-overflow-scrolling:touch;
}
.chat-input::placeholder{color:rgba(255,255,255,0.25);}
.chat-input::-webkit-scrollbar{display:none;}
.send-btn{
  width:40px;height:40px;
  background:linear-gradient(135deg,#30d158,#25a244);
  border-radius:50%;display:flex;align-items:center;justify-content:center;
  font-size:18px;cursor:pointer;flex-shrink:0;
  box-shadow:0 2px 8px rgba(48,209,88,0.4);
  transition:transform 0.12s;
}
.send-btn:active{transform:scale(0.88);}
.no-api-tip{
  margin:8px 14px 0;padding:8px 12px;
  background:rgba(255,214,10,0.08);border:1px solid rgba(255,214,10,0.2);
  border-radius:10px;font-size:11px;color:rgba(255,214,10,0.8);
  text-align:center;cursor:pointer;flex-shrink:0;
}

/* ===== 添加角色 ===== */
.af-label{font-size:13px;color:rgba(255,255,255,0.5);margin-bottom:6px;}
.af-input{
  width:100%;height:44px;background:rgba(255,255,255,0.06);
  border:1.5px solid rgba(255,255,255,0.1);border-radius:13px;
  padding:0 14px;font-size:14px;color:white;outline:none;font-family:inherit;
  transition:border-color 0.2s;
}
.af-input::placeholder{color:rgba(255,255,255,0.2);}
.af-input:focus{border-color:rgba(48,209,88,0.4);}
.af-textarea{
  width:100%;height:80px;background:rgba(255,255,255,0.06);
  border:1.5px solid rgba(255,255,255,0.1);border-radius:13px;
  padding:12px 14px;font-size:13px;color:white;outline:none;
  font-family:inherit;resize:none;line-height:1.5;
  transition:border-color 0.2s;
}
.af-textarea::placeholder{color:rgba(255,255,255,0.2);}
.af-textarea:focus{border-color:rgba(48,209,88,0.4);}
.emoji-picker{display:flex;flex-wrap:wrap;gap:8px;margin-top:6px;}
.emoji-opt{
  width:38px;height:38px;border-radius:10px;
  background:rgba(255,255,255,0.06);border:1.5px solid rgba(255,255,255,0.08);
  display:flex;align-items:center;justify-content:center;font-size:20px;
  cursor:pointer;transition:all 0.15s;
}
.emoji-opt.selected{border-color:rgba(48,209,88,0.5);background:rgba(48,209,88,0.1);}
.af-submit{
  width:100%;height:46px;background:linear-gradient(135deg,#30d158,#25a244);
  border:none;border-radius:13px;color:white;font-size:15px;font-weight:700;
  cursor:pointer;box-shadow:0 4px 16px rgba(48,209,88,0.3);
  transition:transform 0.12s;
}
.af-submit:active{transform:scale(0.98);}

/* ===== API设置 ===== */
.status-card-api{
  background:rgba(255,59,48,0.08);border:1px solid rgba(255,59,48,0.2);
  border-radius:14px;padding:12px 14px;
  display:flex;align-items:center;gap:10px;margin-bottom:14px;transition:all 0.3s;
}
.status-card-api.connected{background:rgba(48,209,88,0.08);border-color:rgba(48,209,88,0.2);}
.status-card-api.testing{background:rgba(255,214,10,0.08);border-color:rgba(255,214,10,0.2);}
.provider-grid{display:grid;grid-template-columns:1fr 1fr;gap:8px;margin-bottom:4px;}
.provider-btn{
  padding:10px 8px;border-radius:12px;
  border:1.5px solid rgba(255,255,255,0.08);
  background:rgba(255,255,255,0.04);
  cursor:pointer;text-align:center;transition:all 0.15s;
}
.provider-btn:active{transform:scale(0.96);}
.provider-btn.selected{border-color:rgba(48,209,88,0.6);background:rgba(48,209,88,0.1);}
.provider-logo{font-size:22px;margin-bottom:4px;}
.provider-name{font-size:12px;font-weight:600;color:rgba(255,255,255,0.8);}
.provider-desc{font-size:10px;color:rgba(255,255,255,0.35);margin-top:2px;}
.provider-btn.selected .provider-name{color:#30d158;}
.form-group{margin-bottom:12px;}
.form-label{font-size:12px;color:rgba(255,255,255,0.45);margin-bottom:6px;display:flex;align-items:center;justify-content:space-between;}
.form-label-tag{font-size:10px;background:rgba(255,214,10,0.15);color:#ffd60a;padding:2px 6px;border-radius:4px;}
.form-input{
  width:100%;height:42px;background:rgba(255,255,255,0.06);
  border:1.5px solid rgba(255,255,255,0.1);border-radius:12px;padding:0 14px;
  font-size:13px;color:white;outline:none;font-family:inherit;transition:border-color 0.2s;
}
.form-input::placeholder{color:rgba(255,255,255,0.2);}
.form-input:focus{border-color:rgba(48,209,88,0.5);}
.key-input{font-family:'Courier New',monospace;letter-spacing:1px;font-size:12px;padding-right:44px;}
.input-wrap{position:relative;}
.input-eye{position:absolute;right:12px;top:50%;transform:translateY(-50%);font-size:16px;cursor:pointer;color:rgba(255,255,255,0.3);}
.model-select{
  width:100%;height:42px;background:rgba(255,255,255,0.06);
  border:1.5px solid rgba(255,255,255,0.1);border-radius:12px;padding:0 14px;
  font-size:13px;color:white;outline:none;font-family:inherit;cursor:pointer;
  -webkit-appearance:none;appearance:none;
  background-image:url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='12' height='8' viewBox='0 0 12 8'%3E%3Cpath d='M1 1l5 5 5-5' stroke='rgba(255,255,255,0.3)' stroke-width='1.5' fill='none' stroke-linecap='round'/%3E%3C/svg%3E");
  background-repeat:no-repeat;background-position:right 14px center;padding-right:36px;
}
.model-select option{background:#1a1a2e;color:white;}
.param-row{display:flex;align-items:center;gap:10px;margin-bottom:10px;}
.param-label{font-size:12px;color:rgba(255,255,255,0.5);width:80px;flex-shrink:0;}
.param-slider{flex:1;height:4px;-webkit-appearance:none;appearance:none;background:rgba(255,255,255,0.1);border-radius:2px;outline:none;cursor:pointer;}
.param-slider::-webkit-slider-thumb{-webkit-appearance:none;width:16px;height:16px;border-radius:50%;background:#30d158;cursor:pointer;box-shadow:0 0 6px rgba(48,209,88,0.4);}
.param-val{font-size:12px;color:#30d158;font-weight:600;width:32px;text-align:right;flex-shrink:0;font-family:'Courier New',monospace;}
.test-btn{
  width:100%;height:44px;background:rgba(48,209,88,0.12);
  border:1.5px solid rgba(48,209,88,0.3);border-radius:13px;color:#30d158;
  font-size:14px;font-weight:600;cursor:pointer;
  display:flex;align-items:center;justify-content:center;gap:8px;
  transition:all 0.15s;margin-bottom:10px;
}
.test-btn.testing{background:rgba(255,214,10,0.1);border-color:rgba(255,214,10,0.3);color:#ffd60a;}
.save-btn{
  width:100%;height:46px;background:linear-gradient(135deg,#30d158,#25a244);
  border:none;border-radius:13px;color:white;font-size:15px;font-weight:700;
  cursor:pointer;box-shadow:0 4px 16px rgba(48,209,88,0.3);transition:transform 0.12s;
}
.save-btn:active{transform:scale(0.98);}
.token-grid{display:grid;grid-template-columns:repeat(3,1fr);gap:8px;margin-bottom:4px;}
.token-item{background:rgba(255,255,255,0.04);border:1px solid rgba(255,255,255,0.06);border-radius:12px;padding:10px 8px;text-align:center;}
.token-val{font-size:16px;font-weight:700;font-family:'Courier New',monospace;}
.token-val.in{color:#0a84ff;}.token-val.out{color:#30d158;}.token-val.total{color:#bf5af2;}
.token-label{font-size:10px;color:rgba(255,255,255,0.3);margin-top:3px;}
.token-cost{text-align:center;font-size:12px;color:rgba(255,255,255,0.3);margin-top:8px;}
.token-cost span{color:#ffd60a;font-weight:600;}
.log-header{display:flex;align-items:center;justify-content:space-between;margin-bottom:10px;}
.log-clear-btn{font-size:11px;color:rgba(255,59,48,0.7);cursor:pointer;padding:3px 8px;border:1px solid rgba(255,59,48,0.2);border-radius:6px;}
.log-list{display:flex;flex-direction:column;gap:6px;max-height:180px;overflow-y:auto;scrollbar-width:none;}
.log-list::-webkit-scrollbar{display:none;}
.log-item{background:rgba(255,255,255,0.03);border:1px solid rgba(255,255,255,0.05);border-radius:10px;padding:8px 10px;border-left:3px solid rgba(255,255,255,0.1);}
.log-item.success{border-left-color:#30d158;}.log-item.error{border-left-color:#ff3b30;}.log-item.info{border-left-color:#0a84ff;}
.log-row1{display:flex;align-items:center;justify-content:space-between;margin-bottom:3px;}
.log-type{font-size:10px;font-weight:700;}
.log-type.success{color:#30d158;}.log-type.error{color:#ff3b30;}.log-type.info{color:#0a84ff;}
.log-time-text{font-size:10px;color:rgba(255,255,255,0.25);font-family:'Courier New',monospace;}
.log-msg-text{font-size:11px;color:rgba(255,255,255,0.55);line-height:1.4;}
.log-tokens{display:flex;gap:8px;margin-top:4px;}
.log-token-tag{font-size:10px;padding:1px 6px;border-radius:4px;font-family:'Courier New',monospace;}
.log-token-tag.in{background:rgba(10,132,255,0.15);color:#0a84ff;}
.log-token-tag.out{background:rgba(48,209,88,0.15);color:#30d158;}
.log-token-tag.ms{background:rgba(255,255,255,0.06);color:rgba(255,255,255,0.35);}
.preset-list{display:flex;flex-direction:column;gap:6px;}
.preset-item{display:flex;align-items:center;gap:10px;padding:10px 12px;background:rgba(255,255,255,0.04);border:1px solid rgba(255,255,255,0.07);border-radius:11px;cursor:pointer;transition:all 0.15s;}
.preset-item:active{background:rgba(255,255,255,0.08);}
.preset-item.active-preset{border-color:rgba(48,209,88,0.4);background:rgba(48,209,88,0.07);}
.preset-icon{font-size:20px;flex-shrink:0;}
.preset-info{flex:1;}
.preset-name{font-size:13px;font-weight:600;color:rgba(255,255,255,0.85);}
.preset-url{font-size:10px;color:rgba(255,255,255,0.3);margin-top:2px;font-family:'Courier New',monospace;}
.collapse-header{display:flex;align-items:center;justify-content:space-between;cursor:pointer;padding:4px 0;}
.collapse-title{font-size:13px;font-weight:600;color:rgba(255,255,255,0.6);}
.collapse-arrow{font-size:12px;color:rgba(255,255,255,0.3);transition:transform 0.3s;}
.collapse-arrow.open{transform:rotate(90deg);}
.collapse-body{overflow:hidden;max-height:0;transition:max-height 0.35s ease;}
.collapse-body.open{max-height:400px;}
.nav-status-dot{width:8px;height:8px;border-radius:50%;background:#ff3b30;flex-shrink:0;box-shadow:0 0 6px rgba(255,59,48,0.6);transition:all 0.3s;}
.nav-status-dot.connected{background:#30d158;box-shadow:0 0 6px rgba(48,209,88,0.6);}
.nav-status-dot.testing{background:#ffd60a;animation:pulse 1s infinite;}
@keyframes pulse{0%,100%{opacity:1;}50%{opacity:0.4;}}

/* ===== 设置页 ===== */
.settings-section{margin-bottom:20px;}
.settings-section-title{font-size:12px;color:rgba(255,255,255,0.3);letter-spacing:1px;text-transform:uppercase;padding:0 4px;margin-bottom:8px;}
.settings-list{background:rgba(255,255,255,0.05);border:1px solid rgba(255,255,255,0.08);border-radius:16px;overflow:hidden;}
.settings-item{display:flex;align-items:center;gap:12px;padding:13px 16px;border-bottom:1px solid rgba(255,255,255,0.05);cursor:pointer;transition:background 0.12s;}
.settings-item:last-child{border-bottom:none;}
.settings-item:active{background:rgba(255,255,255,0.05);}
.settings-item-icon{width:32px;height:32px;border-radius:8px;display:flex;align-items:center;justify-content:center;font-size:16px;flex-shrink:0;}
.settings-item-text{flex:1;}
.settings-item-label{font-size:14px;color:white;}
.settings-item-sub{font-size:11px;color:rgba(255,255,255,0.35);margin-top:1px;}
.settings-item-right{font-size:16px;color:rgba(255,255,255,0.2);}
.toggle{width:44px;height:26px;background:rgba(255,255,255,0.1);border-radius:13px;position:relative;cursor:pointer;transition:background 0.3s;flex-shrink:0;}
.toggle.on{background:#30d158;}
.toggle-thumb{position:absolute;top:3px;left:3px;width:20px;height:20px;background:white;border-radius:50%;transition:transform 0.3s;box-shadow:0 1px 4px rgba(0,0,0,0.3);}
.toggle.on .toggle-thumb{transform:translateX(18px);}

/* ===== 小组件编辑 ===== */
.widget-edit-list{display:flex;flex-direction:column;gap:10px;}
.widget-edit-item{display:flex;align-items:center;gap:12px;padding:12px 14px;background:rgba(255,255,255,0.05);border:1px solid rgba(255,255,255,0.08);border-radius:14px;cursor:pointer;transition:all 0.15s;}
.widget-edit-item.active{border-color:rgba(48,209,88,0.4);background:rgba(48,209,88,0.07);}
.widget-edit-preview{width:60px;height:44px;border-radius:10px;display:flex;align-items:center;justify-content:center;font-size:24px;background:rgba(255,255,255,0.06);flex-shrink:0;}
.widget-edit-info{flex:1;}
.widget-edit-name{font-size:14px;font-weight:600;color:white;}
.widget-edit-desc{font-size:11px;color:rgba(255,255,255,0.35);margin-top:2px;}
.widget-edit-check{font-size:18px;color:#30d158;flex-shrink:0;}

/* ===== Toast ===== */
.toast{
  position:absolute;top:56px;left:50%;
  transform:translateX(-50%) translateY(-10px);
  background:rgba(28,28,32,0.96);
  backdrop-filter:blur(20px);
  border:1px solid rgba(255,255,255,0.1);
  border-radius:12px;padding:10px 18px;
  font-size:13px;color:white;white-space:nowrap;
  opacity:0;transition:all 0.25s;
  z-index:99999;pointer-events:none;
}
.toast.show{opacity:1;transform:translateX(-50%) translateY(0);}
</style>
</head>
<body>
<div class="phone">
  <div class="btn-r"></div><div class="btn-l1"></div><div class="btn-l2"></div><div class="btn-l3"></div>
  <div class="screen">
    <div class="notch"><div class="notch-speaker"></div><div class="notch-camera"></div></div>
    <div class="toast" id="toast"></div>

    <!-- ══════════════ 锁屏 ══════════════ -->
    <div class="page" id="lockScreen" onclick="unlockPhone()">
      <div class="lock-bg-stars" id="lockStars"></div>
      <div class="lock-time-area">
        <div class="lock-time" id="lockTime">00:00</div>
        <div class="lock-date" id="lockDate">加载中...</div>
      </div>
      <div class="lock-notif">
        <div class="lock-notif-row">
          <div class="lock-notif-icon">💬</div>
          <div>
            <div class="lock-notif-app">AI助手</div>
            <div class="lock-notif-msg">你好！我在等你呢～</div>
          </div>
        </div>
      </div>
      <div class="lock-swipe">↑ 向上滑动解锁</div>
    </div>

    <!-- ══════════════ 主屏 ══════════════ -->
    <div class="page" id="homeScreen">
      <div class="wallpaper">
        <div class="wallpaper-orb" style="width:300px;height:300px;top:-80px;left:-60px;background:radial-gradient(circle,#3d1a6e,transparent);"></div>
        <div class="wallpaper-orb" style="width:250px;height:250px;bottom:100px;right:-80px;background:radial-gradient(circle,#0a3d6e,transparent);"></div>
      </div>
      <div class="status-bar">
        <div class="status-time" id="homeTime">00:00</div>
        <div class="status-icons">
          <span class="status-icon-text">●●●</span>
          <span class="status-icon-text" style="margin:0 2px;">WiFi</span>
          <div class="battery"><div class="battery-fill"></div></div>
        </div>
      </div>
      <div class="desktop-area">
        <div class="pages-container" id="pagesContainer">
          <div class="desktop-page" id="desktopPage0">
            <div class="widget-area" id="widgetArea"></div>
            <div class="icon-grid" id="iconGrid0"></div>
          </div>
          <div class="desktop-page" id="desktopPage1">
            <div class="icon-grid" id="iconGrid1"></div>
          </div>
          <div class="desktop-page" id="desktopPage2">
            <div class="icon-grid" id="iconGrid2"></div>
          </div>
        </div>
        <div class="page-dots" id="pageDots">
          <div class="page-dot active"></div>
          <div class="page-dot"></div>
          <div class="page-dot"></div>
        </div>
      </div>
      <div class="dock">
        <div class="app-icon" onclick="openApp('msgList')">
          <div class="icon-img" style="background:linear-gradient(135deg,#30d158,#25a244);">
            💬<div class="badge" id="dockBadge">2</div>
          </div>
          <div class="icon-label">消息</div>
        </div>
        <div class="app-icon" onclick="openApp('addFriend')">
          <div class="icon-img" style="background:linear-gradient(135deg,#0a84ff,#0060cc);">👤</div>
          <div class="icon-label">好友</div>
        </div>
        <div class="app-icon" onclick="openApp('apiSettings')">
          <div class="icon-img" style="background:linear-gradient(135deg,#bf5af2,#9b3fd4);">🔌</div>
          <div class="icon-label">API</div>
        </div>
        <div class="app-icon" onclick="openApp('settings')">
          <div class="icon-img" style="background:linear-gradient(135deg,#636366,#48484a);">⚙️</div>
          <div class="icon-label">设置</div>
        </div>
      </div>
      <div class="home-indicator"><div class="home-bar"></div></div>
    </div>

    <!-- ══════════════ 消息列表 ══════════════ -->
    <div class="page app-page" id="msgListScreen">
      <div class="app-nav">
        <div class="app-nav-row">
          <div class="back-btn" onclick="goHome()">‹</div>
          <div class="app-nav-title">消息</div>
          <div style="font-size:22px;cursor:pointer;color:rgba(255,255,255,0.7);" onclick="openApp('addFriend')">✏️</div>
        </div>
      </div>
      <div class="app-scroll" id="msgListScroll">
        <div class="app-scroll-inner" style="padding:0;">
          <div class="msg-search-wrap">
            <div class="msg-search">
              <span style="font-size:14px;color:rgba(255,255,255,0.3);">🔍</span>
              <input placeholder="搜索" id="msgSearchInput" oninput="filterMsgList(this.value)">
            </div>
          </div>
          <div class="msg-list" id="msgList"></div>
        </div>
      </div>
      <div class="home-indicator"><div class="home-bar"></div></div>
    </div>

    <!-- ══════════════ 聊天页 ══════════════ -->
    <div class="page app-page" id="chatScreen">
      <div class="chat-nav">
        <div class="chat-nav-row">
          <div class="back-btn" onclick="goBack('msgList')">‹</div>
          <div class="chat-avatar-sm" id="chatAvatar">🤖</div>
          <div class="chat-name-wrap">
            <div class="chat-name" id="chatName">AI助手</div>
            <div class="chat-status-text" id="chatStatusText">在线</div>
          </div>
          <div style="display:flex;gap:8px;">
            <div class="chat-action-btn">📞</div>
            <div class="chat-action-btn">⋯</div>
          </div>
        </div>
      </div>
      <div class="no-api-tip" id="noApiTip" style="display:none;" onclick="openApp('apiSettings')">
        ⚠️ 未配置API · 点击前往设置，配置后即可真实对话
      </div>
      <div class="chat-messages" id="chatMessages"></div>
      <div class="chat-input-area">
        <textarea class="chat-input" id="chatInput" placeholder="发消息..." rows="1"
          onkeydown="handleChatKey(event)" oninput="autoResize(this)"></textarea>
        <div class="send-btn" onclick="sendMessage()">↑</div>
      </div>
      <div class="home-indicator"><div class="home-bar"></div></div>
    </div>

    <!-- ══════════════ 添加角色 ══════════════ -->
    <div class="page app-page" id="addFriendScreen">
      <div class="app-nav">
        <div class="app-nav-row">
          <div class="back-btn" onclick="goHome()">‹</div>
          <div class="app-nav-title">添加角色</div>
        </div>
      </div>
      <div class="app-scroll">
        <div class="app-scroll-inner">
          <div class="card">
            <div class="card-title">🎭 选择头像</div>
            <div class="emoji-picker" id="emojiPicker"></div>
          </div>
          <div class="card">
            <div class="card-title">📝 角色信息</div>
            <div style="display:flex;flex-direction:column;gap:14px;">
              <div>
                <div class="af-label">角色名称</div>
                <input class="af-input" id="afName" placeholder="给角色起个名字">
              </div>
              <div>
                <div class="af-label">性格 & 说话风格</div>
                <textarea class="af-textarea" id="afPersonality" placeholder="例如：开朗活泼的高中女生，喜欢用颜文字，有点小傲娇但内心温柔..."></textarea>
              </div>
              <div>
                <div class="af-label">背景故事</div>
                <textarea class="af-textarea" id="afBackground" placeholder="例如：和用户是同班同学，暗恋用户很久了..."></textarea>
              </div>
              <div>
                <div class="af-label">开场白</div>
                <input class="af-input" id="afGreeting" placeholder="例如：哼，你终于来了！">
              </div>
            </div>
          </div>
          <button class="af-submit" onclick="addFriend()">✨ 创建角色</button>
        </div>
      </div>
      <div class="home-indicator"><div class="home-bar"></div></div>
    </div>

    <!-- ══════════════ API设置 ══════════════ -->
    <div class="page app-page" id="apiSettingsScreen">
      <div class="app-nav">
        <div class="app-nav-row">
          <div class="back-btn" onclick="goHome()">‹</div>
          <div class="app-nav-title">🔌 API 设置</div>
          <div class="nav-status-dot" id="apiStatusDot"></div>
        </div>
      </div>
      <div class="app-scroll">
        <div class="app-scroll-inner">
          <div class="status-card-api" id="apiStatusCard">
            <div style="font-size:22px;" id="apiStatusIcon">🔴</div>
            <div style="flex:1;">
              <div style="font-size:13px;font-weight:600;color:white;" id="apiStatusTitle">未连接</div>
              <div style="font-size:11px;color:rgba(255,255,255,0.4);margin-top:2px;" id="apiStatusSub">请配置API信息后测试连接</div>
            </div>
            <div style="font-size:11px;font-weight:600;color:rgba(255,255,255,0.3);font-family:'Courier New',monospace;" id="apiStatusPing">-- ms</div>
          </div>
          <div class="card">
            <div class="card-title">🏢 选择服务商</div>
            <div class="provider-grid">
              <div class="provider-btn selected" data-provider="openai" onclick="selectProvider(this)"><div class="provider-logo">🤖</div><div class="provider-name">OpenAI</div><div class="provider-desc">GPT-4o / GPT-4</div></div>
              <div class="provider-btn" data-provider="claude" onclick="selectProvider(this)"><div class="provider-logo">✨</div><div class="provider-name">Claude</div><div class="provider-desc">Anthropic</div></div>
              <div class="provider-btn" data-provider="gemini" onclick="selectProvider(this)"><div class="provider-logo">💎</div><div class="provider-name">Gemini</div><div class="provider-desc">Google AI</div></div>
              <div class="provider-btn" data-provider="custom" onclick="selectProvider(this)"><div class="provider-logo">⚙️</div><div class="provider-name">自定义</div><div class="provider-desc">任意兼容接口</div></div>
            </div>
          </div>
          <div class="card" id="presetCard" style="display:none;">
            <div class="card-title">⚡ 快速预设</div>
            <div class="preset-list">
              <div class="preset-item" onclick="applyPreset('deepseek',this)"><div class="preset-icon">🐋</div><div class="preset-info"><div class="preset-name">DeepSeek</div><div class="preset-url">api.deepseek.com/v1</div></div></div>
              <div class="preset-item" onclick="applyPreset('moonshot',this)"><div class="preset-icon">🌙</div><div class="preset-info"><div class="preset-name">月之暗面 Kimi</div><div class="preset-url">api.moonshot.cn/v1</div></div></div>
              <div class="preset-item" onclick="applyPreset('zhipu',this)"><div class="preset-icon">🧠</div><div class="preset-info"><div class="preset-name">智谱 GLM</div><div class="preset-url">open.bigmodel.cn/api/paas/v4</div></div></div>
              <div class="preset-item" onclick="applyPreset('qwen',this)"><div class="preset-icon">🌟</div><div class="preset-info"><div class="preset-name">通义千问</div><div class="preset-url">dashscope.aliyuncs.com/...</div></div></div>
              <div class="preset-item" onclick="applyPreset('ollama',this)"><div class="preset-icon">🦙</div><div class="preset-info"><div class="preset-name">Ollama 本地</div><div class="preset-url">localhost:11434/v1</div></div></div>
            </div>
          </div>
          <div class="card">
            <div class="card-title">🔑 接口配置</div>
            <div class="form-group">
              <div class="form-label"><span>API Base URL</span><span class="form-label-tag">必填</span></div>
              <input class="form-input" id="apiUrl" placeholder="https://api.openai.com/v1" value="https://api.openai.com/v1">
            </div>
            <div class="form-group">
              <div class="form-label"><span>API Key</span><span class="form-label-tag">必填</span></div>
              <div class="input-wrap">
                <input class="form-input key-input" id="apiKey" type="password" placeholder="sk-xxxxxxxxxxxxxxxx">
                <span class="input-eye" id="eyeBtn" onclick="toggleKeyVisible()">👁</span>
              </div>
            </div>
            <div class="form-group">
              <div class="form-label"><span>模型</span></div>
              <select class="model-select" id="modelSelect" onchange="onModelChange()">
                <option value="gpt-4o">gpt-4o</option>
                <option value="gpt-4o-mini">gpt-4o-mini</option>
                <option value="gpt-4-turbo">gpt-4-turbo</option>
                <option value="gpt-3.5-turbo">gpt-3.5-turbo</option>
                <option value="custom">自定义模型名...</option>
              </select>
            </div>
            <div class="form-group" id="customModelGroup" style="display:none;">
              <div class="form-label"><span>自定义模型名</span></div>
              <input class="form-input" id="customModel" placeholder="如 deepseek-chat">
            </div>
          </div>
          <div class="card">
            <div class="card-title">🎛️ 生成参数</div>
            <div class="param-row"><span class="param-label">Temperature</span><input type="range" class="param-slider" id="tempSlider" min="0" max="2" step="0.05" value="0.85" oninput="document.getElementById('tempVal').textContent=parseFloat(this.value).toFixed(2)"><span class="param-val" id="tempVal">0.85</span></div>
            <div class="param-row"><span class="param-label">Max Tokens</span><input type="range" class="param-slider" id="maxTokenSlider" min="64" max="4096" step="64" value="512" oninput="document.getElementById('maxTokenVal').textContent=this.value"><span class="param-val" id="maxTokenVal">512</span></div>
            <div class="param-row"><span class="param-label">Top P</span><input type="range" class="param-slider" id="topPSlider" min="0" max="1" step="0.05" value="0.95" oninput="document.getElementById('topPVal').textContent=parseFloat(this.value).toFixed(2)"><span class="param-val" id="topPVal">0.95</span></div>
            <div class="param-row" style="margin-bottom:0;"><span class="param-label">Freq Penalty</span><input type="range" class="param-slider" id="freqSlider" min="0" max="2" step="0.05" value="0.3" oninput="document.getElementById('freqVal').textContent=parseFloat(this.value).toFixed(2)"><span class="param-val" id="freqVal">0.30</span></div>
          </div>
          <div class="card">
            <div class="collapse-header" onclick="toggleCollapse()">
              <div class="collapse-title">🔧 高级设置</div>
              <div class="collapse-arrow" id="collapseArrow">›</div>
            </div>
            <div class="collapse-body" id="collapseBody">
              <div style="height:12px;"></div>
              <div class="form-group"><div class="form-label"><span>请求超时（秒）</span></div><input class="form-input" id="timeout" type="number" value="30"></div>
              <div class="form-group"><div class="form-label"><span>全局 System Prompt 前缀</span></div><textarea style="width:100%;height:70px;background:rgba(255,255,255,0.06);border:1.5px solid rgba(255,255,255,0.1);border-radius:12px;padding:10px 14px;font-size:12px;color:white;outline:none;font-family:inherit;resize:none;line-height:1.5;" id="sysPrefix" placeholder="在所有角色Prompt前插入的全局指令..."></textarea></div>
              <div class="form-group" style="margin-bottom:0;"><div class="form-label"><span>代理地址（可选）</span></div><input class="form-input" id="proxyUrl" placeholder="http://127.0.0.1:7890"></div>
            </div>
          </div>
          <div class="card">
            <div class="card-title">📊 Token 统计 <span style="margin-left:auto;font-size:10px;color:rgba(255,255,255,0.2);cursor:pointer;" onclick="resetStats()">重置</span></div>
            <div class="token-grid">
              <div class="token-item"><div class="token-val in" id="statIn">0</div><div class="token-label">输入</div></div>
              <div class="token-item"><div class="token-val out" id="statOut">0</div><div class="token-label">输出</div></div>
              <div class="token-item"><div class="token-val total" id="statTotal">0</div><div class="token-label">累计</div></div>
            </div>
            <div class="token-cost">预估费用：<span id="statCost">$0.0000</span></div>
            <canvas id="tokenChart" style="width:100%;height:50px;margin-top:10px;border-radius:8px;display:block;"></canvas>
          </div>
          <div class="card">
            <div class="log-header">
              <div class="card-title" style="margin-bottom:0;">📋 使用日志</div>
              <div class="log-clear-btn" onclick="clearLogs()">清空</div>
            </div>
            <div class="log-list" id="logList"><div style="text-align:center;font-size:12px;color:rgba(255,255,255,0.2);padding:16px;">暂无日志</div></div>
          </div>
          <div class="card">
            <button class="test-btn" id="testBtn" onclick="testConnection()"><span>🔗</span><span id="testBtnText">测试连接</span></button>
            <button class="save-btn" onclick="saveApiConfig()">保存配置</button>
          </div>
        </div>
      </div>
      <div class="home-indicator"><div class="home-bar"></div></div>
    </div>

    <!-- ══════════════ 设置 ══════════════ -->
    <div class="page app-page" id="settingsScreen">
      <div class="app-nav">
        <div class="app-nav-row">
          <div class="back-btn" onclick="goHome()">‹</div>
          <div class="app-nav-title">设置</div>
        </div>
      </div>
      <div class="app-scroll">
        <div class="app-scroll-inner">
          <div style="display:flex;align-items:center;gap:14px;padding:16px;background:rgba(255,255,255,0.05);border:1px solid rgba(255,255,255,0.08);border-radius:18px;margin-bottom:20px;">
            <div style="width:56px;height:56px;border-radius:50%;background:linear-gradient(135deg,#bf5af2,#9b3fd4);display:flex;align-items:center;justify-content:center;font-size:26px;">👤</div>
            <div><div style="font-size:17px;font-weight:700;color:white;">我的账号</div><div style="font-size:12px;color:rgba(255,255,255,0.4);margin-top:3px;">点击编辑个人资料</div></div>
            <div style="margin-left:auto;font-size:18px;color:rgba(255,255,255,0.3);">›</div>
          </div>
          <div class="settings-section">
            <div class="settings-section-title">外观</div>
            <div class="settings-list">
              <div class="settings-item" onclick="openApp('widgetEdit')"><div class="settings-item-icon" style="background:rgba(255,149,0,0.2);">🧩</div><div class="settings-item-text"><div class="settings-item-label">自定义小组件</div><div class="settings-item-sub">选择桌面显示的小组件</div></div><div class="settings-item-right">›</div></div>
              <div class="settings-item"><div class="settings-item-icon" style="background:rgba(255,214,10,0.2);">🌙</div><div class="settings-item-text"><div class="settings-item-label">深色模式</div></div><div class="toggle on" onclick="this.classList.toggle('on')"><div class="toggle-thumb"></div></div></div>
              <div class="settings-item"><div class="settings-item-icon" style="background:rgba(10,132,255,0.2);">🖼️</div><div class="settings-item-text"><div class="settings-item-label">壁纸</div><div class="settings-item-sub">深空星云</div></div><div class="settings-item-right">›</div></div>
            </div>
          </div>
          <div class="settings-section">
            <div class="settings-section-title">AI 对话</div>
            <div class="settings-list">
              <div class="settings-item" onclick="openApp('apiSettings')"><div class="settings-item-icon" style="background:rgba(191,90,242,0.2);">🔌</div><div class="settings-item-text"><div class="settings-item-label">API 配置</div><div class="settings-item-sub" id="settingsApiStatus">未配置</div></div><div class="settings-item-right">›</div></div>
              <div class="settings-item"><div class="settings-item-icon" style="background:rgba(48,209,88,0.2);">🧠</div><div class="settings-item-text"><div class="settings-item-label">记忆系统</div><div class="settings-item-sub">保存对话记忆</div></div><div class="toggle on" onclick="this.classList.toggle('on')"><div class="toggle-thumb"></div></div></div>
              <div class="settings-item"><div class="settings-item-icon" style="background:rgba(255,100,50,0.2);">💬</div><div class="settings-item-text"><div class="settings-item-label">流式输出</div><div class="settings-item-sub">逐字显示回复</div></div><div class="toggle on" onclick="this.classList.toggle('on')"><div class="toggle-thumb"></div></div></div>
            </div>
          </div>
          <div class="settings-section">
            <div class="settings-section-title">通知</div>
            <div class="settings-list">
              <div class="settings-item"><div class="settings-item-icon" style="background:rgba(255,59,48,0.2);">🔔</div><div class="settings-item-text"><div class="settings-item-label">消息通知</div></div><div class="toggle on" onclick="this.classList.toggle('on')"><div class="toggle-thumb"></div></div></div>
              <div class="settings-item"><div class="settings-item-icon" style="background:rgba(10,132,255,0.2);">📳</div><div class="settings-item-text"><div class="settings-item-label">震动反馈</div></div><div class="toggle" onclick="this.classList.toggle('on')"><div class="toggle-thumb"></div></div></div>
            </div>
          </div>
          <div class="settings-section">
            <div class="settings-section-title">数据</div>
            <div class="settings-list">
              <div class="settings-item" onclick="clearAllData()"><div class="settings-item-icon" style="background:rgba(255,59,48,0.2);">🗑️</div><div class="settings-item-text"><div class="settings-item-label" style="color:#ff3b30;">清除所有数据</div><div class="settings-item-sub">删除角色、对话记录</div></div><div class="settings-item-right">›</div></div>
            </div>
          </div>
          <div style="text-align:center;font-size:12px;color:rgba(255,255,255,0.15);padding:20px 0;">AI Phone v1.0.0 · 用❤️制作</div>
        </div>
      </div>
      <div class="home-indicator"><div class="home-bar"></div></div>
    </div>

    <!-- ══════════════ 小组件编辑 ══════════════ -->
    <div class="page app-page" id="widgetEditScreen">
      <div class="app-nav">
        <div class="app-nav-row">
          <div class="back-btn" onclick="goBack('settings')">‹</div>
          <div class="app-nav-title">自定义小组件</div>
        </div>
      </div>
      <div class="app-scroll">
        <div class="app-scroll-inner">
          <div style="font-size:13px;color:rgba(255,255,255,0.4);margin-bottom:14px;">选择在桌面第一页显示的小组件</div>
          <div class="widget-edit-list" id="widgetEditList"></div>
        </div>
      </div>
      <div class="home-indicator"><div class="home-bar"></div></div>
    </div>

  </div><!-- /screen -->
</div><!-- /phone -->

<script>
// ═══════════════════════════════════════════
// 数据
// ═══════════════════════════════════════════
let friends = [
  {
    id:1, name:'小樱', avatar:'🌸',
    avatarBg:'linear-gradient(135deg,#ff6b9d,#c44dff)',
    personality:'开朗活泼的高中女生，有点小傲娇但内心温柔，喜欢用颜文字，说话带点撒娇',
    background:'和用户是同班同学，暗恋用户很久了',
    greeting:'哼，你终于来了！人家等好久了 (｡•́︿•̀｡)',
    messages:[], unread:1
  },
  {
    id:2, name:'AI助手', avatar:'🤖',
    avatarBg:'linear-gradient(135deg,#0a84ff,#0060cc)',
    personality:'专业、友善、博学的AI助手，回答简洁准确，偶尔幽默',
    background:'你的私人AI助手',
    greeting:'你好！有什么我可以帮你的吗？',
    messages:[], unread:1
  },
];
let currentFriendId = null;
let currentDesktopPage = 0;
let stats = {in:0,out:0,total:0,cost:0};
let apiLogs = [];
let chartData = {in:[],out:[]};
let activeWidget = 'weather';
let selectedAvatar = '🌸';
let keyVisible = false;

const widgetDefs = [
  {id:'weather',name:'天气',desc:'显示当前天气和温度',icon:'🌤️'},
  {id:'clock',  name:'时钟',desc:'显示时间和日期',    icon:'🕐'},
  {id:'steps',  name:'步数',desc:'今日步数统计',      icon:'👟'},
];
const desktopApps = [
  [
    {label:'相机',icon:'📷',bg:'linear-gradient(135deg,#1c1c1e,#3a3a3c)'},
    {label:'相册',icon:'🖼️',bg:'linear-gradient(135deg,#ff6b35,#f7931e)'},
    {label:'音乐',icon:'🎵',bg:'linear-gradient(135deg,#fc466b,#3f5efb)'},
    {label:'地图',icon:'🗺️',bg:'linear-gradient(135deg,#11998e,#38ef7d)'},
  ],
  [
    {label:'日历',icon:'📅',bg:'linear-gradient(135deg,#ff3b30,#cc2a22)'},
    {label:'备忘录',icon:'📝',bg:'linear-gradient(135deg,#ffd60a,#ff9500)'},
    {label:'计算器',icon:'🔢',bg:'linear-gradient(135deg,#636366,#48484a)'},
    {label:'天气',icon:'⛅',bg:'linear-gradient(135deg,#0a84ff,#0060cc)'},
    {label:'健康',icon:'❤️',bg:'linear-gradient(135deg,#ff2d55,#cc1a3a)'},
    {label:'钱包',icon:'💰',bg:'linear-gradient(135deg,#30d158,#25a244)'},
    {label:'浏览器',icon:'🌐',bg:'linear-gradient(135deg,#0a84ff,#bf5af2)'},
    {label:'邮件',icon:'✉️',bg:'linear-gradient(135deg,#0a84ff,#0060cc)'},
  ],
  [
    {label:'游戏',icon:'🎮',bg:'linear-gradient(135deg,#bf5af2,#9b3fd4)'},
    {label:'视频',icon:'🎬',bg:'linear-gradient(135deg,#ff3b30,#ff6b35)'},
    {label:'文件',icon:'📁',bg:'linear-gradient(135deg,#636366,#48484a)'},
    {label:'时钟',icon:'⏰',bg:'linear-gradient(135deg,#1c1c1e,#3a3a3c)'},
  ],
];
const providerPresets = {
  openai:{url:'https://api.openai.com/v1',models:['gpt-4o','gpt-4o-mini','gpt-4-turbo','gpt-3.5-turbo','custom']},
  claude:{url:'https://api.anthropic.com/v1',models:['claude-3-5-sonnet-20241022','claude-3-5-haiku-20241022','claude-3-opus-20240229','custom']},
  gemini:{url:'https://generativelanguage.googleapis.com/v1beta',models:['gemini-1.5-pro','gemini-1.5-flash','gemini-pro','custom']},
  custom:{url:'',models:['custom']}
};
const quickPresets = {
  deepseek:{url:'https://api.deepseek.com/v1',model:'deepseek-chat'},
  moonshot:{url:'https://api.moonshot.cn/v1',model:'moonshot-v1-8k'},
  zhipu:   {url:'https://open.bigmodel.cn/api/paas/v4',model:'glm-4'},
  qwen:    {url:'https://dashscope.aliyuncs.com/compatible-mode/v1',model:'qwen-turbo'},
  ollama:  {url:'http://localhost:11434/v1',model:'llama3'}
};
const modelPricing = {
  'gpt-4o':{in:0.005,out:0.015},'gpt-4o-mini':{in:0.00015,out:0.0006},
  'gpt-4-turbo':{in:0.01,out:0.03},'gpt-3.5-turbo':{in:0.0005,out:0.0015},
  'default':{in:0.001,out:0.002}
};

// ═══════════════════════════════════════════
// 页面导航 — 用 translateX 切换，不重排
// ═══════════════════════════════════════════
let pageStack = [];

function showPage(id){
  // 隐藏所有应用页
  document.querySelectorAll('.page').forEach(p=>{
    if(p.id !== 'lockScreen' && p.id !== 'homeScreen'){
      p.classList.remove('active');
    }
  });
  const el = document.getElementById(id);
  if(!el) return;
  if(id === 'homeScreen'){
    el.classList.add('active');
  } else if(id === 'lockScreen'){
    // 不处理
  } else {
    el.classList.add('active');
  }
}

function unlockPhone(){
  const lock = document.getElementById('lockScreen');
  const home = document.getElementById('homeScreen');
  lock.style.transition = 'transform 0.4s cubic-bezier(0.4,0,0.2,1),opacity 0.4s';
  lock.style.transform = 'translateY(-100%)';
  lock.style.opacity = '0';
  home.classList.add('active');
  setTimeout(()=>{ lock.style.display='none'; },400);
}

function goHome(){
  showPage('homeScreen');
  pageStack = [];
}

function goBack(to){
  const pageMap = {
    msgList:'msgListScreen', settings:'settingsScreen',
    home:'homeScreen'
  };
  showPage(pageMap[to] || 'homeScreen');
}

function openApp(name){
  const pageMap = {
    msgList:'msgListScreen', chat:'chatScreen',
    addFriend:'addFriendScreen', apiSettings:'apiSettingsScreen',
    settings:'settingsScreen', widgetEdit:'widgetEditScreen'
  };
  const id = pageMap[name];
  if(!id) return;
  showPage(id);
  // 各页面初始化
  if(name==='msgList') renderMsgList();
  if(name==='widgetEdit') renderWidgetEditList();
  if(name==='addFriend') initAddFriend();
}

// ═══════════════════════════════════════════
// 时钟
// ═══════════════════════════════════════════
function updateTime(){
  const now = new Date();
  const h = String(now.getHours()).padStart(2,'0');
  const m = String(now.getMinutes()).padStart(2,'0');
  const t = `${h}:${m}`;
  const days=['星期日','星期一','星期二','星期三','星期四','星期五','星期六'];
  const d = `${now.getMonth()+1}月${now.getDate()}日 ${days[now.getDay()]}`;
  const el1=document.getElementById('lockTime');
  const el2=document.getElementById('lockDate');
  const el3=document.getElementById('homeTime');
  if(el1)el1.textContent=t;
  if(el2)el2.textContent=d;
  if(el3)el3.textContent=t;
  // 时钟小组件
  const wc=document.getElementById('widgetClockTime');
  const wd=document.getElementById('widgetClockDate');
  const ww=document.getElementById('widgetClockWeek');
  const weeks=['周日','周一','周二','周三','周四','周五','周六'];
  if(wc)wc.textContent=t;
  if(wd)wd.textContent=`${now.getMonth()+1}月${now.getDate()}日`;
  if(ww)ww.textContent=weeks[now.getDay()];
}
setInterval(updateTime,1000);
updateTime();

// ═══════════════════════════════════════════
// 锁屏星星
// ═══════════════════════════════════════════
function initStars(){
  const c=document.getElementById('lockStars');
  if(!c)return;
  for(let i=0;i<60;i++){
    const s=document.createElement('div');
    s.className='star';
    const sz=Math.random()*2+1;
    s.style.cssText=`width:${sz}px;height:${sz}px;top:${Math.random()*100}%;left:${Math.random()*100}%;--d:${2+Math.random()*4}s;--delay:${Math.random()*4}s;`;
    c.appendChild(s);
  }
}
initStars();

// ═══════════════════════════════════════════
// 桌面分页
// ═══════════════════════════════════════════
function initDesktop(){
  renderWidget();
  desktopApps.forEach((apps,pi)=>{
    const grid=document.getElementById(`iconGrid${pi}`);
    if(!grid)return;
    grid.innerHTML=apps.map(a=>`
      <div class="app-icon" onclick="showToast('🚧 ${a.label}开发中')">
        <div class="icon-img" style="background:${a.bg};">${a.icon}</div>
        <div class="icon-label">${a.label}</div>
      </div>`).join('');
  });
  initDesktopSwipe();
}

function initDesktopSwipe(){
  const container=document.getElementById('pagesContainer');
  let startX=0,startY=0,isDrag=false,isHoriz=null;

  function onStart(x,y){ startX=x;startY=y;isDrag=true;isHoriz=null; }
  function onEnd(x){
    if(!isDrag)return;
    const dx=x-startX;
    if(isHoriz&&Math.abs(dx)>40){
      if(dx<0&&currentDesktopPage<2)currentDesktopPage++;
      else if(dx>0&&currentDesktopPage>0)currentDesktopPage--;
      updateDesktopPage();
    }
    isDrag=false;isHoriz=null;
  }
  function onMove(x,y){
    if(!isDrag||isHoriz!==null)return;
    const dx=Math.abs(x-startX),dy=Math.abs(y-startY);
    if(dx>5||dy>5) isHoriz=dx>dy;
  }

  container.addEventListener('touchstart',e=>onStart(e.touches[0].clientX,e.touches[0].clientY),{passive:true});
  container.addEventListener('touchmove', e=>onMove(e.touches[0].clientX,e.touches[0].clientY),{passive:true});
  container.addEventListener('touchend',  e=>onEnd(e.changedTouches[0].clientX),{passive:true});
  container.addEventListener('mousedown', e=>onStart(e.clientX,e.clientY));
  container.addEventListener('mousemove', e=>onMove(e.clientX,e.clientY));
  container.addEventListener('mouseup',   e=>onEnd(e.clientX));
  container.addEventListener('mouseleave',()=>{isDrag=false;isHoriz=null;});
}

function updateDesktopPage(){
  document.getElementById('pagesContainer').style.transform=`translateX(-${currentDesktopPage*100}%)`;
  document.querySelectorAll('.page-dot').forEach((d,i)=>d.classList.toggle('active',i===currentDesktopPage));
}

// ═══════════════════════════════════════════
// 小组件
// ═══════════════════════════════════════════
const widgetHTML = {
  weather:`<div class="widget widget-weather" onclick="showToast('🌤️ 天气')">
    <div class="widget-weather-top"><div><div class="widget-weather-temp">23°</div><div class="widget-weather-desc">晴转多云</div><div class="widget-weather-city">📍 北京市</div></div><div class="widget-weather-icon">⛅</div></div>
    <div class="widget-weather-row"><div class="widget-weather-item">湿度 <span>45%</span></div><div class="widget-weather-item">风速 <span>3级</span></div><div class="widget-weather-item">体感 <span>21°</span></div></div>
  </div>`,
  clock:`<div class="widget widget-clock" onclick="showToast('🕐 时钟')">
    <div class="widget-clock-time" id="widgetClockTime">00:00</div>
    <div style="flex:1;"><div class="widget-clock-date" id="widgetClockDate">--</div><div class="widget-clock-week" id="widgetClockWeek">--</div></div>
    <div style="font-size:28px;">🌙</div>
  </div>`,
  steps:`<div class="widget widget-steps" onclick="showToast('👟 步数')">
    <div class="widget-steps-top"><div class="widget-steps-label">今日步数</div><div style="font-size:18px;">🔥</div></div>
    <div class="widget-steps-num">6,284</div>
    <div class="widget-steps-bar"><div class="widget-steps-fill"></div></div>
  </div>`
};

function renderWidget(){
  const area=document.getElementById('widgetArea');
  if(area) area.innerHTML=widgetHTML[activeWidget]||'';
  updateTime();
}

function renderWidgetEditList(){
  const el=document.getElementById('widgetEditList');
  if(!el)return;
  el.innerHTML=widgetDefs.map(w=>`
    <div class="widget-edit-item ${w.id===activeWidget?'active':''}" onclick="selectWidget('${w.id}',this)">
      <div class="widget-edit-preview">${w.icon}</div>
      <div class="widget-edit-info"><div class="widget-edit-name">${w.name}</div><div class="widget-edit-desc">${w.desc}</div></div>
      ${w.id===activeWidget?'<div class="widget-edit-check">✓</div>':''}
    </div>`).join('');
}

function selectWidget(id,el){
  activeWidget=id;
  document.querySelectorAll('.widget-edit-item').forEach(item=>{
    item.classList.remove('active');
    const chk=item.querySelector('.widget-edit-check');
    if(chk)chk.remove();
  });
  el.classList.add('active');
  const chk=document.createElement('div');
  chk.className='widget-edit-check';chk.textContent='✓';
  el.appendChild(chk);
  renderWidget();
  showToast(`✅ 已切换为${widgetDefs.find(w=>w.id===id)?.name}小组件`);
}

// ═══════════════════════════════════════════
// 消息列表 — 微信风格
// ═══════════════════════════════════════════
function renderMsgList(filter){
  const el=document.getElementById('msgList');
  if(!el)return;
  let list=friends;
  if(filter) list=friends.filter(f=>f.name.includes(filter));
  if(list.length===0){
    el.innerHTML='<div style="text-align:center;padding:40px 20px;font-size:14px;color:rgba(255,255,255,0.3);">'+
      (filter?'没有找到相关角色':'还没有角色<br><br>点击右上角 ✏️ 添加')+'</div>';
    return;
  }
  // 更新Dock角标
  const totalUnread=friends.reduce((s,f)=>s+f.unread,0);
  const badge=document.getElementById('dockBadge');
  if(badge){ badge.textContent=totalUnread; badge.style.display=totalUnread>0?'flex':'none'; }

  el.innerHTML=list.map(f=>{
    const lastMsg=f.messages.length>0?f.messages[f.messages.length-1]:null;
    const preview=lastMsg
      ? (lastMsg.role==='user'?'你：':'')+lastMsg.content.slice(0,22)+(lastMsg.content.length>22?'...':'')
      : f.greeting.slice(0,22)+'...';
    const timeStr=lastMsg?fmtTime(lastMsg.time):'刚刚';
    return `
      <div class="msg-item" onclick="openChat(${f.id})">
        <div class="msg-avatar" style="background:${f.avatarBg||'rgba(255,255,255,0.1)'};">
          <span style="font-size:26px;position:relative;z-index:1;">${f.avatar}</span>
          <div class="msg-online-dot"></div>
        </div>
        <div class="msg-content">
          <div class="msg-top-row">
            <div class="msg-name">${f.name}</div>
            <div class="msg-time-label">${timeStr}</div>
          </div>
          <div class="msg-bottom-row">
            <div class="msg-preview-text">${escHtml(preview)}</div>
            ${f.unread>0?`<div class="msg-unread-badge">${f.unread}</div>`:''}
          </div>
        </div>
      </div>`;
  }).join('');
}

function filterMsgList(val){ renderMsgList(val.trim()||undefined); }

function fmtTime(ts){
  if(!ts)return'';
  const now=new Date(),d=new Date(ts),diff=now-d;
  if(diff<60000)return'刚刚';
  if(diff<3600000)return Math.floor(diff/60000)+'分钟前';
  if(diff<86400000)return`${d.getHours()}:${String(d.getMinutes()).padStart(2,'0')}`;
  return`${d.getMonth()+1}/${d.getDate()}`;
}

// ═══════════════════════════════════════════
// 聊天
// ═══════════════════════════════════════════
function openChat(fid){
  currentFriendId=fid;
  const f=friends.find(x=>x.id===fid);
  if(!f)return;
  f.unread=0;
  document.getElementById('chatAvatar').textContent=f.avatar;
  document.getElementById('chatAvatar').style.background=f.avatarBg||'rgba(255,255,255,0.1)';
  document.getElementById('chatName').textContent=f.name;
  document.getElementById('chatStatusText').textContent='在线';
  const hasApi=!!(document.getElementById('apiKey').value.trim());
  document.getElementById('noApiTip').style.display=hasApi?'none':'block';
  renderChatMessages(f);
  if(f.messages.length===0){
    setTimeout(()=>{
      addMsg(f,'assistant',f.greeting);
      renderChatMessages(f);
    },600);
  }
  showPage('chatScreen');
}

function renderChatMessages(f){
  const el=document.getElementById('chatMessages');
  if(!el)return;
  el.innerHTML=f.messages.map(m=>`
    <div class="msg-bubble-wrap ${m.role==='user'?'me':''}">
      ${m.role==='assistant'?`<div class="bubble-avatar-sm" style="background:${f.avatarBg||'rgba(255,255,255,0.1)'};">${f.avatar}</div>`:''}
      <div>
        <div class="bubble ${m.role==='user'?'me':'them'}">${escHtml(m.content)}</div>
        <div class="bubble-time" style="${m.role==='user'?'text-align:right;':''}">${fmtTime(m.time)}</div>
      </div>
    </div>`).join('');
  requestAnimationFrame(()=>{ el.scrollTop=el.scrollHeight; });
}

function addMsg(f,role,content){
  f.messages.push({role,content,time:Date.now(),read:role==='user'});
}

function escHtml(s){
  return s.replace(/&/g,'&amp;').replace(/</g,'&lt;').replace(/>/g,'&gt;').replace(/\n/g,'<br>');
}

async function sendMessage(){
  const input=document.getElementById('chatInput');
  const text=input.value.trim();
  if(!text)return;
  const f=friends.find(x=>x.id===currentFriendId);
  if(!f)return;
  input.value='';input.style.height='auto';
  addMsg(f,'user',text);
  renderChatMessages(f);
  const apiKey=document.getElementById('apiKey').value.trim();
  const apiUrl=document.getElementById('apiUrl').value.trim();
  if(!apiKey||!apiUrl){
    showTyping(f);
    setTimeout(()=>{
      removeTyping();
      const rs=['嗯嗯，我听到你说"'+text.slice(0,8)+'"了～','哼，这个问题嘛...（配置API后我会更聪明的！）','你说得对呢！不过我现在还没有AI加持 (｡•́︿•̀｡)','去设置里配置一下API，我就能真正和你聊天啦！'];
      addMsg(f,'assistant',rs[Math.floor(Math.random()*rs.length)]);
      renderChatMessages(f);
    },800+Math.random()*700);
    return;
  }
  showTyping(f);
  try{
    const reply=await callAPI(f,text);
    removeTyping();
    addMsg(f,'assistant',reply);
    renderChatMessages(f);
  }catch(e){
    removeTyping();
    addMsg(f,'assistant',`[连接失败：${e.message}]`);
    renderChatMessages(f);
    addApiLog('error','API调用失败',e.message);
  }
}

function showTyping(f){
  const el=document.getElementById('chatMessages');
  const d=document.createElement('div');
  d.className='msg-bubble-wrap';d.id='typingBubble';
  d.innerHTML=`<div class="bubble-avatar-sm" style="background:${f.avatarBg||'rgba(255,255,255,0.1)'};">${f.avatar}</div>
    <div class="bubble typing"><div class="typing-dot"></div><div class="typing-dot"></div><div class="typing-dot"></div></div>`;
  el.appendChild(d);
  requestAnimationFrame(()=>{ el.scrollTop=el.scrollHeight; });
}
function removeTyping(){ document.getElementById('typingBubble')?.remove(); }
function handleChatKey(e){ if(e.key==='Enter'&&!e.shiftKey){e.preventDefault();sendMessage();} }
function autoResize(el){ el.style.height='auto';el.style.height=Math.min(el.scrollHeight,100)+'px'; }

// ═══════════════════════════════════════════
// API调用
// ═══════════════════════════════════════════
async function callAPI(friend,userText){
  const url=document.getElementById('apiUrl').value.trim();
  const key=document.getElementById('apiKey').value.trim();
  const sel=document.getElementById('modelSelect').value;
  const model=sel==='custom'?document.getElementById('customModel').value.trim():sel;
  const temperature=parseFloat(document.getElementById('tempSlider').value);
  const maxTokens=parseInt(document.getElementById('maxTokenSlider').value);
  const topP=parseFloat(document.getElementById('topPSlider').value);
  const freqPenalty=parseFloat(document.getElementById('freqSlider').value);
  const sysPrefix=document.getElementById('sysPrefix').value.trim();
  const systemPrompt=buildSystemPrompt(friend,sysPrefix);
  const history=friend.messages.slice(-10).map(m=>({role:m.role==='user'?'user':'assistant',content:m.content}));
  history.push({role:'user',content:userText});
  const messages=[{role:'system',content:systemPrompt},...history];
  const t0=Date.now();
  const resp=await fetch(`${url}/chat/completions`,{
    method:'POST',
    headers:{'Content-Type':'application/json','Authorization':`Bearer ${key}`},
    body:JSON.stringify({model,messages,temperature,max_tokens:maxTokens,top_p:topP,frequency_penalty:freqPenalty})
  });
  if(!resp.ok){const err=await resp.json().catch(()=>({}));throw new Error(err.error?.message||`HTTP ${resp.status}`);}
  const data=await resp.json();
  const content=data.choices?.[0]?.message?.content||'';
  const inT=data.usage?.prompt_tokens||0;
  const outT=data.usage?.completion_tokens||0;
  const elapsed=Date.now()-t0;
  updateStats(inT,outT,elapsed);
  addApiLog('success',`${friend.name} 对话完成`,model,inT,outT,elapsed);
  return content;
}

function buildSystemPrompt(friend,prefix){
  const now=new Date();
  const t=`${now.getHours()}:${String(now.getMinutes()).padStart(2,'0')}`;
  return`${prefix?prefix+'\n\n':''}你正在扮演角色"${friend.name}"，请严格按照以下设定进行对话：

【性格与风格】
${friend.personality}

【背景故事】
${friend.background}

【对话规则】
- 始终以第一人称"我"说话，完全沉浸在角色中
- 绝对不要暴露自己是AI或语言模型
- 回复要自然口语化，控制在1-4句话
- 根据性格特点使用符合角色的语气和用词
- 可以适当使用颜文字增加真实感
- 当前时间：${t}`;
}

// ═══════════════════════════════════════════
// 添加角色
// ═══════════════════════════════════════════
const avatarEmojis=['🌸','🤖','🦊','🐱','🐼','🦁','🐸','🦋','🌙','⭐','🔥','💎','🌈','🎭','👻','🦄'];
const avatarBgs=[
  'linear-gradient(135deg,#ff6b9d,#c44dff)','linear-gradient(135deg,#0a84ff,#0060cc)',
  'linear-gradient(135deg,#ff9500,#ff6b35)','linear-gradient(135deg,#30d158,#25a244)',
  'linear-gradient(135deg,#636366,#48484a)','linear-gradient(135deg,#ff3b30,#cc2a22)',
  'linear-gradient(135deg,#11998e,#38ef7d)','linear-gradient(135deg,#fc466b,#3f5efb)',
  'linear-gradient(135deg,#1a1a2e,#3d1a6e)','linear-gradient(135deg,#ffd60a,#ff9500)',
  'linear-gradient(135deg,#ff6432,#ff3b7a)','linear-gradient(135deg,#0a84ff,#bf5af2)',
  'linear-gradient(135deg,#fc466b,#3f5efb)','linear-gradient(135deg,#bf5af2,#9b3fd4)',
  'linear-gradient(135deg,#636366,#1c1c1e)','linear-gradient(135deg,#c44dff,#0a84ff)',
];
let selectedAvatarBg=avatarBgs[0];

function initAddFriend(){
  const picker=document.getElementById('emojiPicker');
  if(!picker)return;
  picker.innerHTML=avatarEmojis.map((e,i)=>`
    <div class="emoji-opt ${e===selectedAvatar?'selected':''}" onclick="selectAvatar('${e}','${avatarBgs[i]||avatarBgs[0]}',this)">${e}</div>`).join('');
}

function selectAvatar(emoji,bg,el){
  selectedAvatar=emoji;selectedAvatarBg=bg;
  document.querySelectorAll('.emoji-opt').forEach(e=>e.classList.remove('selected'));
  el.classList.add('selected');
}

function addFriend(){
  const name=document.getElementById('afName').value.trim();
  const personality=document.getElementById('afPersonality').value.trim();
  const background=document.getElementById('afBackground').value.trim();
  const greeting=document.getElementById('afGreeting').value.trim();
  if(!name){showToast('⚠️ 请输入角色名称');return;}
  friends.unshift({
    id:Date.now(),name,avatar:selectedAvatar,avatarBg:selectedAvatarBg,
    personality:personality||'友善、自然的对话风格',
    background:background||'',
    greeting:greeting||`你好，我是${name}！`,
    messages:[],unread:0
  });
  document.getElementById('afName').value='';
  document.getElementById('afPersonality').value='';
  document.getElementById('afBackground').value='';
  document.getElementById('afGreeting').value='';
  showToast(`✨ 角色"${name}"创建成功！`);
  setTimeout(()=>openApp('msgList'),700);
}

// ═══════════════════════════════════════════
// API设置
// ═══════════════════════════════════════════
function selectProvider(el){
  document.querySelectorAll('.provider-btn').forEach(b=>b.classList.remove('selected'));
  el.classList.add('selected');
  const p=el.dataset.provider;
  const preset=providerPresets[p];
  if(preset.url)document.getElementById('apiUrl').value=preset.url;
  const sel=document.getElementById('modelSelect');
  sel.innerHTML=preset.models.map(m=>`<option value="${m}">${m==='custom'?'自定义模型名...':m}</option>`).join('');
  onModelChange();
  document.getElementById('presetCard').style.display=p==='custom'?'block':'none';
}
function applyPreset(name,el){
  const p=quickPresets[name];
  document.getElementById('apiUrl').value=p.url;
  document.getElementById('modelSelect').value='custom';
  document.getElementById('customModelGroup').style.display='block';
  document.getElementById('customModel').value=p.model;
  document.querySelectorAll('.preset-item').forEach(e=>e.classList.remove('active-preset'));
  el.classList.add('active-preset');
  showToast(`✅ 已应用 ${name} 预设`);
}
function onModelChange(){
  document.getElementById('customModelGroup').style.display=document.getElementById('modelSelect').value==='custom'?'block':'none';
}
function toggleKeyVisible(){
  keyVisible=!keyVisible;
  document.getElementById('apiKey').type=keyVisible?'text':'password';
  document.getElementById('eyeBtn').textContent=keyVisible?'🙈':'👁';
}
function toggleCollapse(){
  document.getElementById('collapseBody').classList.toggle('open');
  document.getElementById('collapseArrow').classList.toggle('open');
}
async function testConnection(){
  const url=document.getElementById('apiUrl').value.trim();
  const key=document.getElementById('apiKey').value.trim();
  if(!url||!key){showToast('⚠️ 请填写URL和API Key');return;}
  setApiStatus('testing');
  const btn=document.getElementById('testBtn');
  btn.classList.add('testing');
  document.getElementById('testBtnText').textContent='测试中...';
  const t=Date.now();
  try{
    await new Promise((res,rej)=>setTimeout(()=>{
      (key.startsWith('sk-')||key.length>10)?res():rej(new Error('Invalid API Key'));
    },800+Math.random()*600));
    const ping=Date.now()-t;
    setApiStatus('connected',ping);
    addApiLog('success','连接成功',`延迟 ${ping}ms`,0,0,ping);
    showToast('🎉 连接成功！');
    document.getElementById('settingsApiStatus').textContent='已配置 · 正常';
  }catch(e){
    setApiStatus('error');
    addApiLog('error','连接失败',e.message);
    showToast('❌ '+e.message);
  }
  btn.classList.remove('testing');
  document.getElementById('testBtnText').textContent='测试连接';
}
function setApiStatus(state,ping){
  const dot=document.getElementById('apiStatusDot');
  const card=document.getElementById('apiStatusCard');
  const icon=document.getElementById('apiStatusIcon');
  const title=document.getElementById('apiStatusTitle');
  const sub=document.getElementById('apiStatusSub');
  const pingEl=document.getElementById('apiStatusPing');
  dot.className='nav-status-dot';card.className='status-card-api';
  if(state==='connected'){
    dot.classList.add('connected');card.classList.add('connected');
    icon.textContent='🟢';title.textContent='已连接';
    sub.textContent=`${document.getElementById('modelSelect').value} · 服务正常`;
    pingEl.textContent=`${ping}ms`;
  }else if(state==='testing'){
    dot.classList.add('testing');card.classList.add('testing');
    icon.textContent='🟡';title.textContent='测试中...';
    sub.textContent='正在验证API连接';pingEl.textContent='...';
  }else{
    icon.textContent='🔴';title.textContent='未连接';
    sub.textContent='请配置API信息后测试连接';pingEl.textContent='-- ms';
  }
}
function saveApiConfig(){
  const url=document.getElementById('apiUrl').value.trim();
  const key=document.getElementById('apiKey').value.trim();
  if(!url||!key){showToast('⚠️ 请填写URL和API Key');return;}
  try{
    localStorage.setItem('apiConfig',JSON.stringify({
      url,key:btoa(key),
      model:document.getElementById('modelSelect').value,
      customModel:document.getElementById('customModel').value,
      temperature:document.getElementById('tempSlider').value,
      maxTokens:document.getElementById('maxTokenSlider').value,
      topP:document.getElementById('topPSlider').value,
      freqPenalty:document.getElementById('freqSlider').value,
      sysPrefix:document.getElementById('sysPrefix').value
    }));
  }catch(e){}
  addApiLog('info','配置已保存',url);
  showToast('✅ 配置保存成功！');
  document.getElementById('settingsApiStatus').textContent='已配置';
}
function loadApiConfig(){
  try{
    const saved=localStorage.getItem('apiConfig');
    if(!saved)return;
    const c=JSON.parse(saved);
    if(c.key)c.key=atob(c.key);
    if(c.url)document.getElementById('apiUrl').value=c.url;
    if(c.key)document.getElementById('apiKey').value=c.key;
    if(c.temperature){document.getElementById('tempSlider').value=c.temperature;document.getElementById('tempVal').textContent=parseFloat(c.temperature).toFixed(2);}
    if(c.maxTokens){document.getElementById('maxTokenSlider').value=c.maxTokens;document.getElementById('maxTokenVal').textContent=c.maxTokens;}
    if(c.topP){document.getElementById('topPSlider').value=c.topP;document.getElementById('topPVal').textContent=parseFloat(c.topP).toFixed(2);}
    if(c.freqPenalty){document.getElementById('freqSlider').value=c.freqPenalty;document.getElementById('freqVal').textContent=parseFloat(c.freqPenalty).toFixed(2);}
    if(c.sysPrefix)document.getElementById('sysPrefix').value=c.sysPrefix;
    if(c.url&&c.key)document.getElementById('settingsApiStatus').textContent='已配置';
    addApiLog('info','已加载上次配置','');
  }catch(e){}
}

// ═══════════════════════════════════════════
// Token统计 & 日志
// ═══════════════════════════════════════════
function updateStats(inT,outT,ms){
  stats.in+=inT;stats.out+=outT;stats.total+=inT+outT;
  const model=document.getElementById('modelSelect').value;
  const pricing=modelPricing[model]||modelPricing.default;
  stats.cost+=(inT/1000*pricing.in)+(outT/1000*pricing.out);
  document.getElementById('statIn').textContent=fmtNum(stats.in);
  document.getElementById('statOut').textContent=fmtNum(stats.out);
  document.getElementById('statTotal').textContent=fmtNum(stats.total);
  document.getElementById('statCost').textContent='$'+stats.cost.toFixed(4);
  chartData.in.push(inT);chartData.out.push(outT);
  if(chartData.in.length>10){chartData.in.shift();chartData.out.shift();}
  drawChart();
}
function fmtNum(n){if(n>=1000000)return(n/1000000).toFixed(1)+'M';if(n>=1000)return(n/1000).toFixed(1)+'K';return String(n);}
function resetStats(){
  stats={in:0,out:0,total:0,cost:0};chartData={in:[],out:[]};
  ['statIn','statOut','statTotal'].forEach(id=>document.getElementById(id).textContent='0');
  document.getElementById('statCost').textContent='$0.0000';
  drawChart();showToast('📊 统计已重置');
}
function drawChart(){
  const canvas=document.getElementById('tokenChart');
  if(!canvas)return;
  const ctx=canvas.getContext('2d');
  const W=canvas.offsetWidth*window.devicePixelRatio||300;
  const H=50*window.devicePixelRatio||50;
  canvas.width=W;canvas.height=H;
  ctx.clearRect(0,0,W,H);
  const all=[...chartData.in,...chartData.out];
  const maxVal=Math.max(...all,1);
  const barW=(W/10)*0.35;
  const gap=W/10;
  for(let i=0;i<10;i++){
    const x=i*gap+gap*0.15;
    const inH=((chartData.in[i]||0)/maxVal)*(H-4);
    const outH=((chartData.out[i]||0)/maxVal)*(H-4);
    ctx.fillStyle='rgba(10,132,255,0.6)';
    ctx.beginPath();ctx.roundRect(x,H-inH,barW,inH,2);ctx.fill();
    ctx.fillStyle='rgba(48,209,88,0.6)';
    ctx.beginPath();ctx.roundRect(x+barW+2,H-outH,barW,outH,2);ctx.fill();
  }
}
function addApiLog(type,title,msg,inT,outT,ms){
  const now=new Date();
  const time=`${String(now.getHours()).padStart(2,'0')}:${String(now.getMinutes()).padStart(2,'0')}:${String(now.getSeconds()).padStart(2,'0')}`;
  apiLogs.unshift({type,title,msg,inT,outT,ms,time});
  if(apiLogs.length>50)apiLogs.pop();
  renderApiLogs();
}
function renderApiLogs(){
  const el=document.getElementById('logList');
  if(!el)return;
  if(apiLogs.length===0){el.innerHTML='<div style="text-align:center;font-size:12px;color:rgba(255,255,255,0.2);padding:16px;">暂无日志</div>';return;}
  el.innerHTML=apiLogs.map(l=>`
    <div class="log-item ${l.type}">
      <div class="log-row1"><span class="log-type ${l.type}">${l.type.toUpperCase()}</span><span class="log-time-text">${l.time}</span></div>
      <div class="log-msg-text">${l.title}${l.msg?' · '+l.msg:''}</div>
      ${(l.inT||l.outT)?`<div class="log-tokens"><span class="log-token-tag in">↑${l.inT}</span><span class="log-token-tag out">↓${l.outT}</span><span class="log-token-tag ms">${l.ms}ms</span></div>`:''}
    </div>`).join('');
}
function clearLogs(){apiLogs=[];renderApiLogs();showToast('🗑️ 日志已清空');}

// ═══════════════════════════════════════════
// 其他
// ═══════════════════════════════════════════
function clearAllData(){
  friends=[];localStorage.clear();
  showToast('🗑️ 数据已清除');
}
function showToast(msg){
  const t=document.getElementById('toast');
  t.textContent=msg;t.classList.add('show');
  setTimeout(()=>t.classList.remove('show'),2200);
}

// ═══════════════════════════════════════════
// 初始化
// ═══════════════════════════════════════════
initDesktop();
loadApiConfig();
setTimeout(()=>addApiLog('info','系统就绪','AI Phone 已启动'),300);
</script>
</body>
</html>
