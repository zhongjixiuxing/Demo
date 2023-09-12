<template>
  <ion-app>
    <ion-router-outlet />
  </ion-app>
</template>

<script setup lang="ts">
import { IonApp, IonRouterOutlet } from '@ionic/vue';
import { logoChrome } from 'ionicons/icons';

import { io, Manager } from "socket.io-client";
import { default as Isomorphic } from 'isomorphic-ws'
import {WebSocket as SFClient}  from '@wahr/capacitor-websocket-client'

console.log('Isomorphic ------ : ', Isomorphic)
import { CapacitorWebsocket } from '@miaz/capacitor-websocket';
import { Preferences } from '@capacitor/preferences';
import { Capacitor, CapacitorHttp } from '@capacitor/core';
import { default as protobuf } from 'protobufjs';
import { default as pako } from 'pako'
const sURL = 'wss://webcast3-ws-web-lq.douyin.com/webcast/im/push/v2/?app_name=douyin_web&version_code=180800&webcast_sdk_version=1.3.0&update_version_code=1.3.0&compress=gzip&internal_ext=internal_src:dim|wss_push_room_id:7277885418958261050|wss_push_did:7188358506633528844|dim_log_id:20230521093022204E5B327EF20D5CDFC6|fetch_time:1684632622323|seq:1|wss_info:0-1684632622323-0-0|wrds_kvs:WebcastRoomRankMessage-1684632106402346965_WebcastRoomStatsMessage-1684632616357153318&cursor=t-1684632622323_r-1_d-1_u-1_h-1&host=https://live.douyin.com&aid=6383&live_id=1&did_rule=3&debug=false&maxCacheMessageNumber=20&endpoint=live_pc&support_wrds=1&im_path=/webcast/im/fetch/&user_unique_id=7188358506633528844&device_platform=web&cookie_enabled=true&screen_width=1440&screen_height=900&browser_language=zh&browser_platform=MacIntel&browser_name=Mozilla&browser_version=5.0%20(Macintosh;%20Intel%20Mac%20OS%20X%2010_15_7)%20AppleWebKit/537.36%20(KHTML,%20like%20Gecko)%20Chrome/113.0.0.0%20Safari/537.36&browser_online=true&tz_name=Asia/Shanghai&identity=audience&room_id=7277885418958261050&heartbeatDuration=0&signature=00000000'


// startSFSocketIo()
startIsomorphic()
// startSocketIo()
// startSocketIoManager()
// startWebSocket() 

  async function getCookies(url: any) {
    const response = await CapacitorHttp.request({
      url,
      webFetchExtra: {
        credentials: 'include',
      },
      ...{
        headers: {
        'accept': 'text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9',
        'User-Agent': 'Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/107.0.0.0 Safari/537.36',
        'cookie': '__ac_nonce=0638733a400869171be51',
        },
      },
    });

    const cookie = response.headers['Set-Cookie'].substring(0, response.headers['Set-Cookie'].indexOf(';'))

    if (response.headers['Set-Cookie']) {
      await Preferences.set({
        key: 'cookie',
        value: cookie
      });

      alert('cookie: ' + cookie)
    }
  }

  async function startCapacitorWebsocket() {
    await getCookies('https://live.douyin.com/668402325818')
    return
    // const socket: any = {
    //   onmessage: (msg) => {
    //     alert('onmessage inner')
    //   }
    // }
    // alert('startCapacitorWebsocket')
    // await CapacitorWebsocket.addListener('default:message', (msg) => {
    //   alert('message----------')
    //   if (socket.onmessage) {
    //     socket.onmessage(msg);
    //   }
    // });
    // await CapacitorWebsocket.addListener('default:connected', (msg) => {
    //   alert('connected----------')
    // });
    // await CapacitorWebsocket.addListener('default:disconnected', (msg) => {
    //    alert('disconnected----------')
    // });
    // await CapacitorWebsocket.addListener('default:statechange', (msg) => {
    //   alert('statechange----------')
    // });
    //  await CapacitorWebsocket.addListener('default:connecterror', (msg) => {
    //   alert('connecterror----------')
    // });
    //  await CapacitorWebsocket.addListener('default:error', (msg) => {
    //   alert('error----------')
    // });
    // const { value } = await Preferences.get({ key: 'cookie' });
    // alert('value -2- : ' + value)
    // await CapacitorWebsocket.build({
    //   name: 'default',
    //   url: sURL,
    //   // headers: { Cookie: value },
    //   headers: {
    //     'Cookie': value,
    //     'user-agent': 'Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/108.0.0.0 Safari/537.36',
    //   }
    // });

    // await CapacitorWebsocket.applyListeners({ name: 'default' });
    // await CapacitorWebsocket.connect({ name: 'default' });
  }

  async function startSFSocketIo() {
    await SFClient.onOpen({id: 'anxing'}, (message, err) => {
        //do something...
        alert('open..')
        console.log("onOpen event have a bug: ", err?.toString())
    })

    await SFClient.onError({id: 'anxing'}, (err: any) => {
        //do something...
        alert('onError..' + JSON.stringify(err))
        console.log("onOpen event have a bug: ", err?.toString())
    })


    await SFClient.onMessage({id: 'anxing'}, (message, err) => {
        //do something...
        alert('message')
        console.log(`received message content: ${message?.data}`)
    })

    await SFClient.connect({id: 'anxing', url: "wss://webcast3-ws-web-lq.douyin.com/webcast/im/push/v2/?app_name=douyin_web&version_code=180800&webcast_sdk_version=1.3.0&update_version_code=1.3.0&compress=gzip&internal_ext=internal_src:dim|wss_push_room_id:7277531733304544035|wss_push_did:7188358506633528844|dim_log_id:20230521093022204E5B327EF20D5CDFC6|fetch_time:1684632622323|seq:1|wss_info:0-1684632622323-0-0|wrds_kvs:WebcastRoomRankMessage-1684632106402346965_WebcastRoomStatsMessage-1684632616357153318&cursor=t-1684632622323_r-1_d-1_u-1_h-1&host=https://live.douyin.com&aid=6383&live_id=1&did_rule=3&debug=false&maxCacheMessageNumber=20&endpoint=live_pc&support_wrds=1&im_path=/webcast/im/fetch/&user_unique_id=7188358506633528844&device_platform=web&cookie_enabled=true&screen_width=1440&screen_height=900&browser_language=zh&browser_platform=MacIntel&browser_name=Mozilla&browser_version=5.0%20(Macintosh;%20Intel%20Mac%20OS%20X%2010_15_7)%20AppleWebKit/537.36%20(KHTML,%20like%20Gecko)%20Chrome/113.0.0.0%20Safari/537.36&browser_online=true&tz_name=Asia/Shanghai&identity=audience&room_id=7277531733304544035&heartbeatDuration=0&signature=00000000"})

    // setTimeout(async () => {
    //     await SFClient.send({data: "hello world!"})
    // }, 2000);
  }

  async function startIsomorphic() {
    await startCapacitorWebsocket()
    const ws = new Isomorphic(sURL);

    ws.onopen = function open() {
      console.log('connected');
      alert('connected')
      // ws.send(Date.now());
    };

    ws.onclose = function close() {
      alert('disconnected')
      console.log('disconnected');
    };

    ws.onmessage = function incoming(data: any) {
      console.log('data --- : ', data)

      // alert('message-----' + JSON.stringify(Object.keys(data)))
      console.log(`Roundtrip time: ${Date.now() - data.data} ms`);

      transferMsgContent(data.data)

      // setTimeout(function timeout() {
      //   ws.send(Date.now());
      // }, 500);
    };
  }

  function startSocketIo() {
    const socket = io("wss://webcast3-ws-web-lq.douyin.com/?app_name=douyin_web&version_code=180800&webcast_sdk_version=1.3.0&update_version_code=1.3.0&compress=gzip&internal_ext=internal_src:dim|wss_push_room_id:7277531733304544035|wss_push_did:7188358506633528844|dim_log_id:20230521093022204E5B327EF20D5CDFC6|fetch_time:1684632622323|seq:1|wss_info:0-1684632622323-0-0|wrds_kvs:WebcastRoomRankMessage-1684632106402346965_WebcastRoomStatsMessage-1684632616357153318&cursor=t-1684632622323_r-1_d-1_u-1_h-1&host=https://live.douyin.com&aid=6383&live_id=1&did_rule=3&debug=false&maxCacheMessageNumber=20&endpoint=live_pc&support_wrds=1&im_path=/webcast/im/fetch/&user_unique_id=7188358506633528844&device_platform=web&cookie_enabled=true&screen_width=1440&screen_height=900&browser_language=zh&browser_platform=MacIntel&browser_name=Mozilla&browser_version=5.0%20(Macintosh;%20Intel%20Mac%20OS%20X%2010_15_7)%20AppleWebKit/537.36%20(KHTML,%20like%20Gecko)%20Chrome/113.0.0.0%20Safari/537.36&browser_online=true&tz_name=Asia/Shanghai&identity=audience&room_id=7277531733304544035&heartbeatDuration=0&signature=00000000", {
      reconnectionDelayMax: 10000,
      transports: ["websocket"],
      path: '/webcast/im/push/v2/',
      // 'forceNew': true,
      //  autoUnref: true,
      //  forceBase64: true
    });

console.log('socket --- : ', socket)

    socket.on("open", () => {
      console.log('open ================ ')
    });

    socket.io.on("error", (e) => { console.log('error ------ ', e) });
    socket.io.on("ping", () => { console.log('ping ------ ') });
    socket.io.on("reconnect", (e) => { console.log('reconnect ------ ') });
    socket.io.on("reconnect_attempt", (e) => { console.log('reconnect_attempt ------ ') });
    socket.io.on("reconnect_error", (e) => { console.log('reconnect_error ------ ') });
    socket.io.on("reconnect_failed", () => { console.log('reconnect_failed ------ ') });


    socket.on("message", () => { console.log('message ------ ') });
    socket.on("connect", () => { console.log('connect ------ ') });
    socket.on("data", () => { console.log('data ------ ') });
    socket.on("connect_error", (err) => {
      // revert to classic upgrade
      socket.io.opts.transports = ["polling", "websocket"];
      console.log('connect_error ------ ', err)
    });
    	socket.on('heartbeat', function(data) {
			console.info("Got hearbeat");
			console.info(data);
		})
		socket.on('connecting', function(data) {
			console.info("connecting");
			console.info(data);
		})

    socket.on('message', function(data) {
			console.info("message");
			console.info(data);
		})


    socket.on('open', function(data) {
			console.info("open");
			console.info(data);
		})
    socket.io.on('open', function() {
			console.info("open2");
		})
  }

   function startSocketIoManager() {


    const manager = new Manager("wss://webcast3-ws-web-lq.douyin.com/webcast/im/push/v2/?app_name=douyin_web&version_code=180800&webcast_sdk_version=1.3.0&update_version_code=1.3.0&compress=gzip&internal_ext=internal_src:dim|wss_push_room_id:7277531733304544035|wss_push_did:7188358506633528844|dim_log_id:20230521093022204E5B327EF20D5CDFC6|fetch_time:1684632622323|seq:1|wss_info:0-1684632622323-0-0|wrds_kvs:WebcastRoomRankMessage-1684632106402346965_WebcastRoomStatsMessage-1684632616357153318&cursor=t-1684632622323_r-1_d-1_u-1_h-1&host=https://live.douyin.com&aid=6383&live_id=1&did_rule=3&debug=false&maxCacheMessageNumber=20&endpoint=live_pc&support_wrds=1&im_path=/webcast/im/fetch/&user_unique_id=7188358506633528844&device_platform=web&cookie_enabled=true&screen_width=1440&screen_height=900&browser_language=zh&browser_platform=MacIntel&browser_name=Mozilla&browser_version=5.0%20(Macintosh;%20Intel%20Mac%20OS%20X%2010_15_7)%20AppleWebKit/537.36%20(KHTML,%20like%20Gecko)%20Chrome/113.0.0.0%20Safari/537.36&browser_online=true&tz_name=Asia/Shanghai&identity=audience&room_id=7277531733304544035&heartbeatDuration=0&signature=00000000", {
        autoConnect: false,
        transports: ["websocket", 'polling'],
        path: '/webcast/im/push/v2/'
      });

      const socket = manager.socket("", {
      });

      manager.open((err) => {
        console.log('err ---- : ', err)
        if (err) {
          // an error has occurred
        } else {
          // the connection was successfully established
        }
      });

    socket.on("open", () => {
      console.log('open ================ ')
    });

    socket.io.on("error", (e) => { console.log('error ------ ', e) });
    socket.io.on("ping", () => { console.log('ping ------ ') });
    socket.io.on("reconnect", (e) => { console.log('reconnect ------ ') });
    socket.io.on("reconnect_attempt", (e) => { console.log('reconnect_attempt ------ ') });
    socket.io.on("reconnect_error", (e) => { console.log('reconnect_error ------ ') });
    socket.io.on("reconnect_failed", () => { console.log('reconnect_failed ------ ') });


    socket.on("message", () => { console.log('message ------ ') });
    socket.on("connect", () => { console.log('connect ------ ') });
    socket.on("data", () => { console.log('data ------ ') });
    socket.on("connect_error", (err) => {
      // revert to classic upgrade
      socket.io.opts.transports = ["polling", "websocket"];
      console.log('connect_error ------ ', err)
    });
    	socket.on('heartbeat', function(data) {
			console.info("Got hearbeat");
			console.info(data);
		})
		socket.on('connecting', function(data) {
			console.info("connecting");
			console.info(data);
		})

    socket.on('message', function(data) {
			console.info("message");
			console.info(data);
		})


    socket.on('open', function(data) {
			console.info("open");
			console.info(data);
		})
    socket.io.on('open', () => {
			console.info("open2");
		})

    
  }

  function startWebSocket() {
    const wss = new WebSocket('wss://webcast3-ws-web-lq.douyin.com/webcast/im/push/v2/?app_name=douyin_web&version_code=180800&webcast_sdk_version=1.3.0&update_version_code=1.3.0&compress=gzip&internal_ext=internal_src:dim|wss_push_room_id:7277531733304544035|wss_push_did:7188358506633528844|dim_log_id:20230521093022204E5B327EF20D5CDFC6|fetch_time:1684632622323|seq:1|wss_info:0-1684632622323-0-0|wrds_kvs:WebcastRoomRankMessage-1684632106402346965_WebcastRoomStatsMessage-1684632616357153318&cursor=t-1684632622323_r-1_d-1_u-1_h-1&host=https://live.douyin.com&aid=6383&live_id=1&did_rule=3&debug=false&maxCacheMessageNumber=20&endpoint=live_pc&support_wrds=1&im_path=/webcast/im/fetch/&user_unique_id=7188358506633528844&device_platform=web&cookie_enabled=true&screen_width=1440&screen_height=900&browser_language=zh&browser_platform=MacIntel&browser_name=Mozilla&browser_version=5.0%20(Macintosh;%20Intel%20Mac%20OS%20X%2010_15_7)%20AppleWebKit/537.36%20(KHTML,%20like%20Gecko)%20Chrome/113.0.0.0%20Safari/537.36&browser_online=true&tz_name=Asia/Shanghai&identity=audience&room_id=7277531733304544035&heartbeatDuration=0&signature=00000000')
    wss.binaryType = "arraybuffer"
    wss.addEventListener("open" , function (e) {
        alert('connected douyin websocket server')
        console.log("open" ,e)
    })

    wss.addEventListener("message" , function (e) {
        console.log("message" ,e.data)
    })

    wss.addEventListener("close" , function (e) {
        alert('close'+JSON.stringify(e))
        console.log("close" ,e)
    })

    wss.addEventListener("error" , function (e) {
        alert('error' + e)
        console.log("error=>" ,e)
    })
  }


  const transferMsgContent = async (buffer: any) =>  {
      // 定义要加载的 .proto 文件 URL
    const protoContent = `syntax = "proto3";

package douyin;


message Response {
  repeated Message messagesList = 1;
  string cursor = 2;
  uint64 fetchInterval = 3;
  uint64 now = 4;
  string internalExt = 5;
  uint32 fetchType = 6;
  map<string, string> routeParams = 7;
  uint64 heartbeatDuration = 8;
  bool needAck = 9;
  string pushServer = 10;
  string liveCursor = 11;
  bool historyNoMore = 12;
}

message Message{
    string method = 1;
    bytes payload = 2;
    int64 msgId = 3;
    int32 msgType = 4;
    int64 offset = 5;
    bool needWrdsStore = 6;
    int64 wrdsVersion = 7;
    string wrdsSubKey = 8;
}

// 聊天
message ChatMessage {
  Common common = 1;
  User user = 2;
  string content = 3;
  bool visibleToSender = 4;
  Image backgroundImage = 5;
  string fullScreenTextColor = 6;
  Image backgroundImageV2 = 7;
  PublicAreaCommon publicAreaCommon = 8;
  Image giftImage = 9;
  uint64 agreeMsgId = 11;
  uint32 priorityLevel = 12;
  LandscapeAreaCommon landscapeAreaCommon = 13;
  uint64 eventTime = 15;
  bool sendReview = 16;
  bool fromIntercom = 17;
  bool intercomHideUserCard = 18;
//  repeated chatTagsList = 19;
  string chatBy  = 20;
  uint32 individualChatPriority = 21;
  Text rtfContent = 22;
}


message LandscapeAreaCommon {
  bool showHead = 1;
  bool showNickname = 2;
  bool showFontColor = 3;
  repeated string colorValueList = 4;
  repeated CommentTypeTag commentTypeTagsList = 5;
}

message RoomUserSeqMessage {
  Common common = 1;
  repeated RoomUserSeqMessageContributor ranksList = 2;
  int64 total = 3;
  string popStr = 4;
  repeated RoomUserSeqMessageContributor seatsList = 5;
  int64 popularity = 6;
  int64 totalUser = 7;
  string totalUserStr = 8;
  string totalStr = 9;
  string onlineUserForAnchor = 10;
  string totalPvForAnchor = 11;
  string upRightStatsStr = 12;
  string upRightStatsStrComplete = 13;

}

message CommonTextMessage {
  Common common = 1;
  User user = 2;
  string scene = 3;
}

message UpdateFanTicketMessage {
  Common common = 1;
  string roomFanTicketCountText = 2;
  uint64 roomFanTicketCount = 3;
  bool forceUpdate = 4;
}

message RoomUserSeqMessageContributor {
  uint64 score = 1;
  User user = 2;
  uint64 rank  = 3;
  uint64 delta  = 4;
  bool isHidden = 5;
  string scoreDescription = 6;
  string exactlyScore = 7;
}

// 礼物消息
message GiftMessage {
  Common common = 1;
  uint64 giftId = 2;
  uint64 fanTicketCount = 3;
  uint64 groupCount = 4;
  uint64 repeatCount = 5;
  uint64 comboCount = 6;
  User user = 7;
  User toUser = 8;
  uint32 repeatEnd = 9;
  TextEffect textEffect = 10;
  uint64 groupId = 11;
  uint64 incomeTaskgifts = 12;
  uint64 roomFanTicketCount = 13;
  GiftIMPriority priority = 14;
  GiftStruct gift = 15;
  string logId = 16;
  uint64 sendType = 17;
  PublicAreaCommon publicAreaCommon = 18;
  Text trayDisplayText = 19;
  uint64 bannedDisplayEffects = 20;
//  GiftTrayInfo trayInfo = 21;
//  AssetEffectMixInfo assetEffectMixInfo = 22;
  bool displayForSelf = 25;
  string interactGiftInfo = 26;
  string diyItemInfo = 27;
  repeated uint64 minAssetSetList = 28;
  uint64 totalCount = 29;
  uint32 clientGiftSource = 30;
//  AnchorGiftData anchorGift = 31;
  repeated uint64 toUserIdsList = 32;
  uint64 sendTime = 33;
  uint64 forceDisplayEffects = 34;
  string traceId = 35;
  uint64 effectDisplayTs = 36;
}

message GiftStruct {
  Image image = 1;
  string describe = 2;
  bool notify = 3;
  uint64 duration = 4;
  uint64 id = 5;
//  GiftStructFansClubInfo fansclubInfo = 6;
  bool forLinkmic = 7;
  bool doodle = 8;
  bool forFansclub = 9;
  bool combo = 10;
  uint32 type = 11;
  uint32 diamondCount = 12;
  bool isDisplayedOnPanel = 13;
  uint64 primaryEffectId = 14;
  Image giftLabelIcon = 15;
  string name = 16;
  string region = 17;
  string manual = 18;
  bool forCustom = 19;
  //  specialEffectsMap = 20;
  Image icon = 21;
  uint32 actionType = 22;
  // fixme 后面的就不写了还有几十个属性

}

message GiftIMPriority {
    repeated uint64 queueSizesList = 1;
    uint64 selfQueuePriority = 2;
    uint64 priority = 3;
}

message TextEffect {
  TextEffectDetail portrait = 1;
  TextEffectDetail landscape = 2;
}

message TextEffectDetail {
  Text text = 1;
  uint32 textFontSize = 2;
  Image background = 3;
  uint32 start = 4;
  uint32 duration = 5;
  uint32 x = 6;
  uint32 y = 7;
  uint32 width = 8;
  uint32 height = 9;
  uint32 shadowDx = 10;
  uint32 shadowDy = 11;
  uint32 shadowRadius = 12;
  string shadowColor = 13;
  string strokeColor = 14;
  uint32 strokeWidth = 15;
}

// 成员消息
message MemberMessage {
  Common common = 1;
  User user = 2;
  uint64 memberCount = 3;
  User operator = 4;
  bool isSetToAdmin = 5;
  bool isTopUser = 6;
  uint64 rankScore = 7;
  uint64 topUserNo = 8;
  uint64 enterType = 9;
  uint64 action = 10;
  string actionDescription = 11;
  uint64 userId = 12;
  EffectConfig effectConfig = 13;
  string popStr = 14;
  EffectConfig enterEffectConfig = 15;
  Image backgroundImage = 16;
  Image backgroundImageV2 = 17;
  Text anchorDisplayText = 18;
  PublicAreaCommon publicAreaCommon = 19;
  uint64 userEnterTipType = 20;
  uint64 anchorEnterTipType = 21;
}


message PublicAreaCommon {
  Image userLabel = 1;
  uint64 userConsumeInRoom = 2;
  uint64 userSendGiftCntInRoom = 3;
}

message EffectConfig {
  uint64 type = 1;
  Image icon = 2;
  uint64 avatarPos = 3;
  Text text = 4;
  Image textIcon = 5;
  uint32 stayTime = 6;
  uint64 animAssetId = 7;
  Image badge = 8;
  repeated uint64 flexSettingArrayList = 9;
  Image textIconOverlay = 10;
  Image animatedBadge = 11;
  bool hasSweepLight = 12;
  repeated uint64 textFlexSettingArrayList = 13;
  uint64 centerAnimAssetId = 14;
  Image dynamicImage = 15;
  map<string,string> extraMap = 16;
  uint64 mp4AnimAssetId = 17;
  uint64 priority = 18;
  uint64 maxWaitTime = 19;
  string dressId = 20;
  uint64 alignment = 21;
  uint64 alignmentOffset = 22;
}

message Text {
  string key = 1;
  string defaultPatter = 2;
  TextFormat defaultFormat = 3;
  repeated TextPiece piecesList = 4;
}

message TextPiece {
  bool type = 1;
  TextFormat format = 2;
  string stringValue = 3;
  TextPieceUser userValue = 4;
  TextPieceGift giftValue = 5;
  TextPieceHeart heartValue = 6;
  TextPiecePatternRef patternRefValue = 7;
  TextPieceImage imageValue = 8;
}


message TextPieceImage {
  Image image = 1;
  float scalingRate = 2;
}

message TextPiecePatternRef {
  string key = 1;
  string defaultPattern = 2;
}

message TextPieceHeart {
  string color = 1;
}

message TextPieceGift {
  uint64 giftId = 1;
  PatternRef nameRef = 2;
}

message PatternRef {
  string key = 1;
  string defaultPattern = 2;
}

message TextPieceUser {
  User user = 1;
  bool withColon = 2;
}

message TextFormat {
  string color = 1;
  bool bold = 2;
  bool italic = 3;
  uint32 weight = 4;
  uint32 italicAngle = 5;
  uint32 fontSize = 6;
  bool useHeighLightColor = 7;
  bool useRemoteClor = 8;
}

// 点赞
message LikeMessage {
  Common common = 1;
  uint64 count = 2;
  uint64 total = 3;
  uint64 color = 4;
  User user = 5;
  string icon = 6;
  DoubleLikeDetail doubleLikeDetail = 7;
  DisplayControlInfo displayControlInfo = 8;
  uint64 linkmicGuestUid = 9;
  string scene = 10;
  PicoDisplayInfo picoDisplayInfo = 11;

}

message SocialMessage {
  Common common = 1;
  User user = 2;
  uint64 shareType = 3;
  uint64 action = 4;
  string shareTarget = 5;
  uint64 followCount = 6;
  PublicAreaCommon publicAreaCommon = 7;
}

message PicoDisplayInfo {
  uint64 comboSumCount = 1;
  string emoji = 2;
  Image emojiIcon = 3;
  string emojiText = 4;
}

message DoubleLikeDetail {
  bool doubleFlag = 1;
  uint32 seqId = 2;
  uint32 renewalsNum = 3;
  uint32 triggersNum = 4;
}

message DisplayControlInfo {
  bool showText = 1;
  bool showIcons = 2;
}

message EpisodeChatMessage {
   Message common = 1;
   User user = 2;
   string content = 3;
   bool visibleToSende = 4;
//   BackgroundImage backgroundImage = 5;
//   PublicAreaCommon publicAreaCommon = 6;
   Image giftImage = 7;
   uint64 agreeMsgId = 8;
   repeated string colorValueList = 9;
}


message MatchAgainstScoreMessage {
  Common common = 1;
  Against against = 2;
  uint32 matchStatus = 3;
  uint32 displayStatus = 4;
}

message Against {
  string leftName = 1;
  Image leftLogo = 2;
  string leftGoal = 3;
//  LeftPlayersList leftPlayersList = 4;
//  LeftGoalStageDetail leftGoalStageDetail = 5;
  string rightName = 6;
  Image rightLogo = 7;
  string rightGoal = 8;
//  RightPlayersList rightPlayersList  = 9;
//  RightGoalStageDetail rightGoalStageDetail = 10;
  uint64 timestamp = 11;
  uint64 version = 12;
  uint64 leftTeamId = 13;
  uint64 rightTeamId = 14;
  uint64 diffSei2absSecond = 15;
  uint32 finalGoalStage = 16;
  uint32 currentGoalStage  =17;
  uint32 leftScoreAddition  =18;
  uint32 rightScoreAddition  =19;
  uint64 leftGoalInt = 20;
  uint64 rightGoalInt = 21;
}

message Common {
  string method = 1;
  uint64 msgId = 2;
  uint64 roomId = 3;
  uint64 createTime = 4;
  uint32 monitor = 5;
  bool isShowMsg = 6;
  string describe = 7;
//  DisplayText displayText = 8;
  uint64 foldType = 9;
  uint64 anchorFoldType = 10;
  uint64 priorityScore = 11;
  string logId = 12;
  string msgProcessFilterK = 13;
  string msgProcessFilterV = 14;
  User user = 15;
//  Room room = 16;
  uint64 anchorFoldTypeV2  = 17;
  uint64 processAtSeiTimeMs  = 18;
  uint64 randomDispatchMs  = 19;
  bool isDispatch = 20;
  uint64 channelId = 21;
  uint64 diffSei2absSecond = 22;
  uint64 anchorFoldDuration = 23;
}

message User {
  uint64 id = 1;
  uint64 shortId = 2;
  string nickName = 3;
  uint32 gender = 4;
  string Signature = 5;
  uint32 Level = 6;
  uint64 Birthday = 7;
  string Telephone = 8;
  Image AvatarThumb = 9;
  Image AvatarMedium = 10;
  Image AvatarLarge = 11;
  bool Verified = 12;
  uint32 Experience = 13;
  string city = 14;
  int32 Status = 15;
  uint64 CreateTime = 16;
  uint64 ModifyTime = 17;
  uint32 Secret = 18;
  string ShareQrcodeUri = 19;
  uint32 IncomeSharePercent = 20;
  repeated Image BadgeImageList = 21;
  FollowInfo FollowInfo = 22;
//  PayGrade PayGrade = 23;
//  FansClub FansClub = 24;
//  Border Border = 25;
  string SpecialId = 26;
  Image AvatarBorder = 27;
  Image Medal = 28;
  repeated Image RealTimeIconsList = 29;
  string displayId = 38;
  string secUid = 46;
  uint64 fanTicketCount = 1022;
  string idStr = 1028;
  uint32 ageRange = 1045;
}

message FollowInfo {
  uint64 followingCount = 1;
  uint64 followerCount = 2;
  uint64 followStatus = 3;
  uint64 pushStatus = 4;
  string remarkName = 5;
  string followerCountStr = 6;
  string followingCountStr = 7;

}
message Image {
  repeated string urlListList = 1;
  string uri = 2;
  uint64 height = 3;
  uint64 width = 4;
  string avgColor = 5;
  uint32 imageType = 6;
  string openWebUrl = 7;
  ImageContent content = 8;
  bool isAnimated = 9;
  NinePatchSetting FlexSettingList = 10;
  NinePatchSetting TextSettingList = 11;
}

message NinePatchSetting {
  repeated string settingListList = 1;
}

message ImageContent {
  string name = 1;
  string fontColor = 2;
  uint64 level = 3;
  string alternativeText = 4;
}

message PushFrame {
  uint64 seqId = 1;
  uint64 logId = 2;
  uint64 service = 3;
  uint64 method = 4;
  repeated HeadersList headersList = 5;
  string payloadEncoding = 6;
  string payloadType = 7;
  bytes payload = 8;

}

message kk {
  uint32 k=14;
}

message SendMessageBody {
  string conversationId = 1;
  uint32 conversationType = 2;
  uint64 conversationShortId = 3;
  string content = 4;
  repeated ExtList ext = 5;
  uint32 messageType = 6;
  string ticket = 7;
  string clientMessageId = 8;

}

message ExtList {
  string key = 1;
  string value = 2;
}

message Rsp{
  int32 a = 1;
  int32 b = 2;
  int32 c = 3;
  string d = 4;
  int32 e = 5;
  message F {
    uint64 q1 = 1;
    uint64 q3 = 3;
    string q4 = 4;
    uint64 q5 = 5;
  }
  F f = 6;
  string g = 7;
  uint64 h = 10;
  uint64 i = 11;
  uint64 j = 13;
}

message PreMessage {
  uint32 cmd = 1;
  uint32 sequenceId = 2;
  string sdkVersion = 3;
  string token = 4;
  uint32 refer = 5;
  uint32 inboxType = 6;
  string buildNumber = 7;
  SendMessageBody sendMessageBody = 8;
  // 字段名待定
  string aa = 9;
  string devicePlatform = 11;
  repeated HeadersList headers = 15;
  uint32 authType = 18;
  string biz = 21;
  string access = 22;
}

message HeadersList {
  string key = 1;
  string value = 2;
}

message LiveShoppingMessage {
  Common common = 1;
  int32 msgType = 2;
  int64  promotionId = 4;


}

message RoomStatsMessage {
  Common common = 1;
  string displayShort = 2;
  string displayMiddle = 3;
  string displayLong = 4;
  int64  displayValue = 5;
  int64  displayVersion = 6;
  bool incremental = 7;
  bool isHidden = 8;
  int64 total = 9;
  int64 displayType = 10;
}

enum CommentTypeTag {
  COMMENTTYPETAGUNKNOWN = 0;
  COMMENTTYPETAGSTAR = 1;
}

message ProductInfo {
  int64 promotionId = 1;
  int32 index = 2;
  repeated int64 targetFlashUidsList = 3;
  int64 explainType = 4;
}
message CategoryInfo {
  int32 id = 1;
  string name = 2;
  repeated int64 promotionIdsList = 3;
  string type = 4;
  string uniqueIndex = 5;
}

message ProductChangeMessage {
  Common common = 1;
  int64 updateTimestamp = 2;
  string updateToast = 3;
  repeated ProductInfo updateProductInfoList = 4;
  int64 total = 5;
  repeated CategoryInfo updateCategoryInfoList = 8;
}`;
    // 解析 .proto 文件中的消息结构
    const root = await protobuf.parse(protoContent).root;
    const ResponseProto = root.lookupType('douyin.Response');
    const PushFrameProto = root.lookupType('douyin.PushFrame');
    const ChatMessageProto = root.lookupType('douyin.ChatMessage');
    const MatchAgainstScoreMessageProto = root.lookupType('douyin.MatchAgainstScoreMessage');
    const WebcastLikeMessageProto = root.lookupType('douyin.LikeMessage');
    const WebcastMemberMessageProto = root.lookupType('douyin.MemberMessage');
    const WebcastGiftMessageProto = root.lookupType('douyin.GiftMessage');
    const WebcastChatMessageProto = root.lookupType('douyin.ChatMessage');
    const WebcastSocialMessageProto = root.lookupType('douyin.SocialMessage');
    const WebcastRoomUserSeqMessageProto = root.lookupType('douyin.RoomUserSeqMessage');
    const WebcastUpdateFanTicketMessageProto = root.lookupType('douyin.UpdateFanTicketMessage');
    const WebcastCommonTextMessageProto = root.lookupType('douyin.CommonTextMessage');
    const WebcastProductChangeMessageProto = root.lookupType('douyin.ProductChangeMessage');
    const data = new Uint8Array(await buffer.arrayBuffer());
    const message: any = PushFrameProto.decode(data);
    const respBody: any = ResponseProto.decode(pako.inflate(message.payload))

    for (const msg of respBody.messagesList) {
      let msgContent
      alert('method: '+msg.method)
      if (msg.method === 'WebcastMatchAgainstScoreMessage') {
        msgContent = MatchAgainstScoreMessageProto.decode(msg.payload)
      } else if (msg.method === 'WebcastLikeMessage') {
        msgContent = WebcastLikeMessageProto.decode(msg.payload)
      } else if (msg.method === 'WebcastMemberMessage') {
        msgContent = WebcastMemberMessageProto.decode(msg.payload)
      } else if (msg.method === 'WebcastGiftMessage') {
        msgContent = WebcastGiftMessageProto.decode(msg.payload)
      } else if (msg.method === 'WebcastChatMessage') {
          msgContent = ChatMessageProto.decode(msg.payload)
      } else if (msg.method === 'WebcastSocialMessage') {
        msgContent = WebcastSocialMessageProto.decode(msg.payload)
      } else if (msg.method === 'WebcastRoomUserSeqMessage') {
        msgContent = WebcastRoomUserSeqMessageProto.decode(msg.payload)
      } else if (msg.method === 'WebcastUpdateFanTicketMessage') {
        msgContent = WebcastUpdateFanTicketMessageProto.decode(msg.payload)
      } else if (msg.method === 'WebcastCommonTextMessage') {
        msgContent = WebcastCommonTextMessageProto.decode(msg.payload)
      } else if (msg.method === 'WebcastProductChangeMessage') {
        msgContent = WebcastProductChangeMessageProto.decode(msg.payload)
      } else {
        console.log('unknown msg method: ', msg)
        continue
      }

      // console.log('msgContent --- : ', msgContent)
    }
  }

</script>