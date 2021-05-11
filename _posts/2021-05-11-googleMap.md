---
title: "Google Maps JavaScript API 사용하기"
categories:
  - api
tags:
  - GoogleMaps
  - Javascript
  - API
toc: true
toc_sticky: true
---

Google Maps Platform 에서 제공하고 있는 지도 API 사용을 했다.


### 1. 맵키 발급하기
구글맵 API를 사용하기 위해서는 해당 프로젝트를 생성하면 **맵키** 를 받을 수 있다.
(사용자 인증 결제 계정 생성 필요)

- Google Maps Platform 
[https://cloud.google.com/maps-platform](https://cloud.google.com/maps-platform)


### 2. 지도 스타일 변경
Google Maps Platform - 지도 스타일 영역에서 사용하고자 하는 지도 스타일을 설정 할 수 있다.


### 3. 지도 Javascript 적용하기

튜토리얼 중에서 지도 Marker Clustering 함께 사용했다.


- Marker Clustering 이란? 
[https://developers.google.com/maps/documentation/javascript/marker-clustering](https://developers.google.com/maps/documentation/javascript/marker-clustering)


```javascript
<script src="https://maps.googleapis.com/maps/api/js?&key=발급받은 KEY값&sensor=false&region=KR"></script>
<script src="/ko/resource/js/map_data.js"></script>
<script src="/ko/resource/js/plugin/markerclusterer.min.js"></script>

// html push
function setNameCardInfo(){
    for (var i = 0, size = markerNameInfo.length; i < size; i++) {
        $(".business_list").append(markerNameInfo[i].html);
    }
}

function initMap() {
    // 처음 중심 좌표
    var center = new google.maps.LatLng(39.932870, 116.331562);
    var map = new google.maps.Map(document.getElementById("map"), {
        zoom: 3,
        center: center,
        mapId : '맵 스타일 KEY값', // 맵 스타일 적용
    });

    var markers = [];
    var customicon = '/images/common/ico_map_pin.png';

    // map
    for (var i = 0, size = data.length; i < size; i++) {
        var mapData = data[i];
        var _html = '';

        _html += '<li data-type="' + mapData.type + '">';
            _html += '<div class="nameCard_info '+ mapData.type +'">';
                _html += '<p class="country">' + mapData.nation + '</p>';
                _html += '<p class="name">' + mapData.name + '</p>';
                _html += '<ul class="user_info">';
                    _html += '<li class="ico ico_phone">' + mapData.phone + '</li>';
                    _html += '<li class="ico ico_mail">' + mapData.mail + '</li>';
                _html += '</ul>';
            _html += '</div>';
                _html += '<div class="division">';
                _html += '<p class="txt">' + mapData.typeStr + '</p>';
                _html += '<a href="javascript:void(0);" class="btn">문의</a>';
            _html += '</div>';
        _html += '</li>';

        var latLng = new google.maps.LatLng(
            mapData.latitude,
            mapData.longitude
        );
        var marker = new google.maps.Marker({
            position: latLng, // 마커의 위치
            icon: customicon, // 마커 아이콘
            map: map, // 마커를 표시할 지도
        });

        markerNameInfo.push({html : _html});
        markers.push(marker);
    }

    var mcOptions = { // markerCluster option
        gridSize: 40,
        maxZoom: 15,
        styles: [
            {
                width: 40,
                height: 40,
                url: "/images/common/ico_mc1.png",
            }
        ]
    };
    var markerCluster = new MarkerClusterer(map, markers, mcOptions);

    setNameCardInfo();
}

google.maps.event.addDomListener(window, "load", initMap);

```


### 4. 작업 중 이슈사항

- Maps API를 로드하면 애플리케이션 동작의 기본 편중이 미국으로 적용되는데 지도 현지화를 위하여 <code>region=KR</code> 추가하여 기본 동작을 재정의하여 동해와 독도로 표기 하였다.
- 지도 스타일에서 회색으로 지도 설정해도 우리나라 스타일이 반영되지 않는다. 
국내법상 지도 데이터를 해외 데이터 센터로 반출할 수 없는 제약이 있어서 구글 지도에서 제공하는 일부 기능이 동작하지 않는데, 여기에 지도 이미지를 동적으로 변경하는 기능도 포함된다.
한국 지도는 국내 서버를 통해 제한적인 스타일의 지도만 서비스하고 있고, 그 외 API 등을 통한 지도 이미지의 변경은 현재로서는 어렵다.