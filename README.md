# GoogleMapFlutterSetBounds
      
      
### Show all markers in GoogleMap view

#### Keep all markers in view with Flutter Google Maps plugin
 

      import 'package:google_maps_flutter/google_maps_flutter.dart';

        class MapUtils {
           static LatLngBounds boundsFromLatLngList(List<LatLng> list) {
            double? x0, x1, y0, y1;
            for (LatLng latLng in list) {
              if (x0 == null) {
                x0 = x1 = latLng.latitude;
                y0 = y1 = latLng.longitude;
              } else {
                if (latLng.latitude > x1!) x1 = latLng.latitude;
                if (latLng.latitude < x0) x0 = latLng.latitude;
                if (latLng.longitude > y1!) y1 = latLng.longitude;
                if (latLng.longitude < y0!) y0 = latLng.longitude;
              }
            }
            return LatLngBounds(northeast: LatLng(x1!, y1!), southwest: LatLng(x0!, y0!));
          }
        }
        
        
#### And you can use this in your GoogleMap as shown below,

##### global vars
         
         
          Completer<GoogleMapController> _controller = Completer();
          late GoogleMapController _googleMapController;

          Iterable markers = [];

##### and in your build(context)


              GoogleMap(
                  gestureRecognizers: Set()
                         ..add(Factory<PanGestureRecognizer>(
                              () => PanGestureRecognizer())),
                        initialCameraPosition:
                            CameraPosition(target: LatLng(11.1271, 78.6569), zoom: 10),
                        zoomControlsEnabled: true,
                        onMapCreated: (GoogleMapController controller) {
                          _controller.complete(controller);
                          _googleMapController = controller;
                          _googleMapController.getVisibleRegion();

                          Set<Marker> _markers = Set();

                          _markers = Set.from(
                            markers,
                          );
                          Future.delayed(
                              Duration(milliseconds: 200),
                              () => controller.animateCamera(
                                  CameraUpdate.newLatLngBounds(
                                      MapUtils.boundsFromLatLngList(
                                          _markers.map((loc) => loc.position).toList()),
                                      1)));
                        },
                        markers: Set.from(
                          markers,
                        ),
                      ),
