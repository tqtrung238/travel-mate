<template>
  <div> 
    <el-row style="margin-top: 2%; margin-left: 30%;">
      <el-button @click="updateLocation" icon="el-icon-star-off" type="primary"> Detect your current location </el-button>
      <el-button @click="showPOI" icon="el-icon-check" type="success"> Show POI </el-button>
    </el-row>

    <div ref="map" id="map"></div>
  </div> 
</template>

<script>
import L from 'leaflet'
import 'leaflet/dist/leaflet.css'


export default {
  name: "POI-recommend",
  data() {
    return {
      map: null,
      marker: null,
      poiList: [] // Array to store points of interest
    }
  },
  mounted() {
    this.loadMap()
  },
  methods: {
    loadMap() {
      // Set the default view to a specific location
      const defaultLatitude = 21.0382394
      const defaultLongitude = 105.7636242
      // Create a map instance
      this.map = L.map(this.$refs.map).setView([defaultLatitude, defaultLongitude], 13)

      // Add a tile layer to the map using OpenStreetMap API
      L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
        attribution: 'Map data &copy; <a href="https://www.openstreetmap.org/">OpenStreetMap</a> contributors',
        maxZoom: 18,
      }).addTo(this.map)
    },
    updateLocation() {
      // Get the current location coordinates using browser's Geolocation API
      navigator.geolocation.getCurrentPosition(position => {
        const latitude = position.coords.latitude
        const longitude = position.coords.longitude

        const redIcon = L.icon({
          iconUrl:
            "https://cdn.rawgit.com/pointhi/leaflet-color-markers/master/img/marker-icon-2x-red.png",
          iconSize: [25, 41],
          iconAnchor: [12, 41],
          popupAnchor: [1, -34],
        });

        if (this.marker != null) {
        } else {
          console.log("Marker is not initialized yet");
        }

        // Add a marker for the current location
        this.marker = L.marker([latitude, longitude], {
          icon: redIcon,
        }).addTo(this.map)  
          .bindPopup('You are here!')
          .openPopup()

        // Remove previous marker if it exists
        // if (this.marker) {
        //   this.map.removeLayer(this.marker)
        // }

        // Pan the map to the current location
        this.map.panTo([latitude, longitude])
      }, error => {
        console.error(error);
      })
    },
    showPOI() {
      // Clear previous markers from the map
      if (this.poiList.length > 0) {
        this.poiList.forEach(marker => {
          this.map.removeLayer(marker);
        });
        this.poiList = []; // Clear the poiList array
      }

      // Sample list of POI coordinates with messages
      const poiCoordinates = [
        { latitude: 21.034059, longitude: 105.8480619, message: 'Khu phố cổ Hà Nội' },
        { latitude: 21.0281175, longitude: 105.8330943, message: 'Văn miếu - Quốc tử giám' },
        { latitude: 21.0580308, longitude: 105.8032223, message: 'Hồ Tây' },
        { latitude: 21.0352181, longitude: 105.8376701, message: 'Hoàng thành Thăng Long' },
        { latitude: 20.9788876, longitude: 105.9135159, message: 'Làng gốm Bát tràng' },
        { latitude: 21.0253297, longitude: 105.8439032, message: 'Nhà tù Hỏa lò' },
        { latitude: 21.032321, longitude: 105.7864628, message: 'Công viên Cầu Giấy' },
        { latitude: 21.0353672, longitude: 105.7819012, message: 'Bảo tàng dân tộc học Việt Nam' },
        { latitude: 21.0021981, longitude: 105.8329531, message: 'Bảo tàng phòng không - Không quân Việt Nam' },
        { latitude: 21.0080054, longitude: 105.8383406, message: 'Công viên Thống Nhất' },
        { latitude: 20.9997736, longitude: 105.8382723, message: 'Chợ mơ' },
        { latitude: 20.9986517, longitude: 105.8396026, message: 'Chùa Hưng Ký' },
        { latitude: 21.0099123, longitude: 105.8431607, message: 'Chợ trời' },
        { latitude: 20.988247, longitude: 105.846693, message: 'Chùa Hoàng Mai' },
        { latitude: 20.9880128, longitude: 105.8487655, message: 'Công Viên hồ Đền Lừ' },
        // Add more locations as needed
      ];

      const poiIcon = L.icon({
        iconUrl:
          'https://cdn.rawgit.com/pointhi/leaflet-color-markers/master/img/marker-icon-2x-blue.png',
        iconSize: [25, 41],
        iconAnchor: [12, 41],
        popupAnchor: [1, -34],
      });

      poiCoordinates.forEach(coord => {
        const marker = L.marker([coord.latitude, coord.longitude], {
          icon: poiIcon,
        }).addTo(this.map);

        // Add the marker to the poiList array
        this.poiList.push(marker);

        // Create a popup message for each POI marker
        marker.bindPopup(coord.message);
        marker.on('click', () => {
          marker.openPopup();
        });
      });
    }
  }
}
</script>

<style>
#map {
  height: 600px;
  width: 100%;
}
</style>