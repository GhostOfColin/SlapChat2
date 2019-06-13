# SlapChat

# [SlapChat](https://slapchat2.herokuapp.com/)

AetherBnB, is an AiBnB clone that enables users to book vacation rentals and view available spots on a responsive map/list of properties.

# Technologies

* [GraphQL](https://graphql.org)
* [MongoDB](https://www.mongodb.com/)
* [Express.js](https://expressjs.com/)
* [React.js](https://reactjs.org)
* [Node.js](https://nodejs.org/)

# Images



# Code Highlights

```js
class SpotMap extends React.Component {

  componentDidMount() {
    let mapOptions = {
      center: this.props.searchParams.location,
      zoom: 14
    };

    let map = this.refs.map;
    window.map = this.map = new google.maps.Map(map, mapOptions);
    window.markerManager = this.MarkerManager = new MarkerManager(this.map);
    this.MarkerManager.updateMarkers(this.props.spots);
    this.map.addListener('bounds_changed', () => {
      let mapBounds = this.map.getBounds();
      let southWest = mapBounds.getSouthWest();
      let northEast = mapBounds.getNorthEast();
      let bounds = { sw: { lat: southWest.lat(), lng: southWest.lng() }, 
      ne: { lat: northEast.lat(), lng: northEast.lng() }};
      this.props.receiveBounds(bounds);

      let location = this.map.getCenter();
      this.props.receiveLocation(location);
    });
  }

  componentDidUpdate(prevProps, prevState) {
    if (!_.isEqual(prevProps.searchParams.location, this.props.searchParams.location)) {
      this.MarkerManager.updateMarkers(this.props.spots);
    } 
  }

  render() {
    return (
    <div id="map-container" ref="map"> </div>
    );
  }
}
```
* Implementing the Google map component, with logic for handling refreshes based on the changing of the bounds of the map.

```js  handleSelect = address => {
    this.setState({ address });
    if (window.map) {
      window.preventFetch = true;
      geocodeByAddress(address).then(results => {
        getLatLng(results[0]).then(({lat, lng}) => {
          window.locationObj = new google.maps.LatLng(lat, lng);
          window.map.panTo(window.locationObj);
        });
        this.activateSearch();
      });
    } else {
      window.preventFetch = false;
      geocodeByAddress(address)
        .then(results => getLatLng(results[0]))
        .then(latLng => {
          const location = latLng;
          this.props.receiveLocation(new google.maps.LatLng(location.lat, location.lng));
          this.activateSearch();
        })
        .catch(error => console.error('Error', error));
    }
  };

```
* Incorporating Google's geocoding functionality into a React search bar and refining the behavior of the Google Map component to update the bounds and associated spots.

## Authors

* **Colin Reitman**
* **Chris Meurer**
* **Valery Nguyen**
