This is a paper comparing several commercially available Geolocation databases to GPS data, which dramatically accurate.

Main findings:
- [[GPS]] reports are a credible groundtruth of IP locations.
- [[MaxMind]]'s [[GeoIP2]] service provides the lowest median error among tested services in NYC.
- IP geolocation performs better on fixed-line consumer networks and universities and worse on mobile broadband and businesses.
- The physical size of subnets is correlated with the accuracy with which they are IP geolocated.
- On a two-month time-scale, the median fixed-line /24 IPv4 subnet in US cities moves less than 1km. 

### Reference


- James Saxon, Nick Feamster. _GPS-Based Geolocation of Consumer IP Addresses_. [arXiv](https://arxiv.org/pdf/2105.13389.pdf)