# Revent Streaming Infrastructure 📺

**Docker-based RTMP Server with HLS and WebRTC Support**

This directory contains the streaming infrastructure for the Revent platform, providing real-time video streaming capabilities through RTMP ingestion, HLS distribution, and WebRTC integration. Built with nginx-rtmp-module and configured for optimal live streaming performance.

## 🎯 Features

### 🎥 Multi-Protocol Streaming
- **RTMP Ingestion**: Accept streams from OBS, FFmpeg, and other broadcasting tools
- **HLS Output**: Adaptive bitrate streaming for web and mobile clients
- **WebRTC Integration**: Real-time peer-to-peer streaming with low latency
- **WHIP Protocol**: WebRTC-HTTP ingestion for browser-based streaming

### 📊 Stream Management
- **Multi-bitrate Support**: Automatic transcoding to multiple quality levels
- **Stream Recording**: Optional recording of live streams to disk
- **Statistics API**: Real-time streaming statistics and viewer counts
- **Authentication**: Stream key validation and access control

### 🌐 Global Distribution
- **CDN Ready**: Optimized for CDN distribution and edge caching
- **Scalable Architecture**: Horizontal scaling with load balancing
- **Cross-platform**: Works with web browsers, mobile apps, and streaming software

## 🏗️ Architecture

```
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│   OBS Studio    │    │  Browser/App    │    │   Mobile App    │
│   (RTMP Push)   │    │  (WebRTC/WHIP) │    │   (HLS Player)  │
└─────────┬───────┘    └─────────┬───────┘    └─────────┬───────┘
          │                      │                      │
          ▼                      ▼                      ▼
┌─────────────────────────────────────────────────────────────────┐
│                    nginx-rtmp Server                            │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐             │
│  │    RTMP     │  │   WebRTC    │  │     HLS     │             │
│  │  :1935      │  │   :8889     │  │   :8888     │             │
│  └─────────────┘  └─────────────┘  └─────────────┘             │
└─────────────────────────────────────────────────────────────────┘
          │                      │                      │
          ▼                      ▼                      ▼
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│   Recording     │    │   Statistics    │    │   CDN/Edge      │
│   Storage       │    │   Dashboard     │    │   Distribution  │
└─────────────────┘    └─────────────────┘    └─────────────────┘
```

## 🚀 Quick Start

### Prerequisites
- Docker and Docker Compose installed
- Available ports: 1935 (RTMP), 8888 (HLS), 8889 (WebRTC)
- Sufficient disk space for stream recording (optional)

### Installation

1. **Navigate to the streaming directory**
   ```bash
   cd nginx-rtmp
   ```

2. **Start the streaming server**
   ```bash
   docker-compose up -d
   ```

3. **Verify the server is running**
   ```bash
   # Check container status
   docker-compose ps
   
   # Check logs
   docker-compose logs -f
   ```

4. **Test the streaming endpoints**
   ```bash
   # Check HLS endpoint
   curl http://localhost:8888/hls/test.m3u8
   
   # Check statistics
   curl http://localhost:8888/stat
   ```

## 🛠️ Configuration

### Docker Compose Configuration

The `docker-compose.yml` file configures:

```yaml
version: '3.8'
services:
  nginx-rtmp:
    build: .
    ports:
      - "1935:1935"   # RTMP ingestion
      - "8888:8888"   # HLS output
      - "8889:8889"   # WebRTC/WHIP
    volumes:
      - ./hls:/var/www/html/hls  # HLS files
      - ./recordings:/recordings  # Stream recordings
    environment:
      - STREAM_KEY_VALIDATION=true
      - RECORDING_ENABLED=false
      - MAX_CONNECTIONS=1000
```

### nginx Configuration

The `nginx.conf` file contains the streaming configuration:

```nginx
# RTMP configuration
rtmp {
    server {
        listen 1935;
        chunk_size 4096;
        
        application live {
            live on;
            
            # HLS output
            hls on;
            hls_path /var/www/html/hls;
            hls_fragment 3;
            hls_playlist_length 60;
            
            # Recording (optional)
            record all;
            record_path /recordings;
            record_unique on;
            record_suffix .flv;
            
            # Stream authentication
            on_publish http://localhost:8888/auth;
        }
    }
}

# HTTP configuration for HLS and WebRTC
http {
    server {
        listen 8888;
        
        # HLS streaming
        location /hls {
            types {
                application/vnd.apple.mpegurl m3u8;
                video/mp2t ts;
            }
            root /var/www/html;
            add_header Cache-Control no-cache;
            add_header Access-Control-Allow-Origin *;
        }
        
        # Statistics endpoint
        location /stat {
            rtmp_stat all;
            rtmp_stat_stylesheet stat.xsl;
        }
        
        # Authentication endpoint
        location /auth {
            return 200;
        }
    }
    
    # WebRTC/WHIP server
    server {
        listen 8889;
        
        # WHIP endpoint for WebRTC ingestion
        location ~ ^/([^/]+)/whip$ {
            # WebRTC ingestion logic
            proxy_pass http://webrtc-backend;
        }
    }
}
```

## 🎛️ Stream Management

### RTMP Streaming (OBS/FFmpeg)

**OBS Studio Configuration:**
- Server: `rtmp://your-server:1935/live`
- Stream Key: `your-stream-key`

**FFmpeg Command:**
```bash
ffmpeg -i input.mp4 -c:v libx264 -c:a aac -f flv rtmp://localhost:1935/live/your-stream-key
```

### WebRTC Streaming (Browser)

**JavaScript WebRTC Setup:**
```javascript
// Create peer connection with WHIP endpoint
const pc = new RTCPeerConnection({
  iceServers: [{ urls: 'stun:stun.l.google.com:19302' }]
});

// Add media stream
const stream = await navigator.mediaDevices.getUserMedia({
  video: true,
  audio: true
});

stream.getTracks().forEach(track => {
  pc.addTrack(track, stream);
});

// Connect to WHIP endpoint
const offer = await pc.createOffer();
await pc.setLocalDescription(offer);

const response = await fetch('http://localhost:8889/your-stream-key/whip', {
  method: 'POST',
  headers: { 'Content-Type': 'application/sdp' },
  body: offer.sdp
});

const answer = await response.text();
await pc.setRemoteDescription({ type: 'answer', sdp: answer });
```

### HLS Playback

**HTML Video Player:**
```html
<video id="video" controls>
  <source src="http://localhost:8888/hls/your-stream-key.m3u8" type="application/vnd.apple.mpegurl">
</video>

<script>
  const video = document.getElementById('video');
  
  if (video.canPlayType('application/vnd.apple.mpegurl')) {
    video.src = 'http://localhost:8888/hls/your-stream-key.m3u8';
  } else if (Hls.isSupported()) {
    const hls = new Hls();
    hls.loadSource('http://localhost:8888/hls/your-stream-key.m3u8');
    hls.attachMedia(video);
  }
</script>
```

## 📊 Monitoring & Analytics

### Statistics API

Access real-time streaming statistics:

```bash
# Get all statistics
curl http://localhost:8888/stat

# Get XML format for parsing
curl http://localhost:8888/stat?format=xml
```

**Example Statistics Output:**
```xml
<rtmp>
  <nginx_version>1.21.6</nginx_version>
  <server>
    <application>
      <name>live</name>
      <live>
        <stream>
          <name>stream-key</name>
          <time>12345</time>
          <bw_in>1024000</bw_in>
          <bw_out>2048000</bw_out>
          <bytes_in>1048576</bytes_in>
          <bytes_out>2097152</bytes_out>
          <clients>5</clients>
        </stream>
      </live>
    </application>
  </server>
</rtmp>
```

### Custom Metrics

Integrate with monitoring systems:

```javascript
// Fetch streaming metrics
async function getStreamingMetrics() {
  const response = await fetch('http://localhost:8888/stat');
  const stats = await response.text();
  
  // Parse XML and extract metrics
  const parser = new DOMParser();
  const xml = parser.parseFromString(stats, 'text/xml');
  
  return {
    activeStreams: xml.querySelectorAll('stream').length,
    totalViewers: Array.from(xml.querySelectorAll('clients'))
      .reduce((sum, el) => sum + parseInt(el.textContent), 0),
    bandwidth: xml.querySelector('bw_out')?.textContent || '0'
  };
}
```

## 🔧 Performance Tuning

### nginx Optimization

```nginx
# Worker process optimization
worker_processes auto;
worker_connections 1024;

# Buffer sizes
client_body_buffer_size 128k;
client_max_body_size 10m;

# Timeouts
client_body_timeout 12;
client_header_timeout 12;
send_timeout 10;

# Gzip compression
gzip on;
gzip_vary on;
gzip_min_length 10240;
gzip_types text/plain text/css application/json application/javascript;
```

### HLS Optimization

```nginx
# HLS fragment settings
hls_fragment 2;          # 2-second fragments for low latency
hls_playlist_length 30;  # 30-second playlist window
hls_sync 100ms;          # Synchronization threshold

# Multiple bitrate outputs
exec ffmpeg -i rtmp://localhost/live/$name
  -c:v libx264 -preset veryfast -tune zerolatency
  -b:v 1000k -maxrate 1000k -bufsize 2000k -s 854x480
  -f flv rtmp://localhost/hls/${name}_480p
  
  -c:v libx264 -preset veryfast -tune zerolatency  
  -b:v 2500k -maxrate 2500k -bufsize 5000k -s 1280x720
  -f flv rtmp://localhost/hls/${name}_720p;
```

## 🚀 Production Deployment

### Docker Production Configuration

```yaml
# docker-compose.prod.yml
version: '3.8'
services:
  nginx-rtmp:
    image: revent/nginx-rtmp:latest
    restart: unless-stopped
    ports:
      - "1935:1935"
      - "8888:8888"
      - "8889:8889"
    volumes:
      - hls_data:/var/www/html/hls
      - recordings_data:/recordings
      - ./nginx.prod.conf:/etc/nginx/nginx.conf:ro
    environment:
      - NGINX_WORKER_PROCESSES=auto
      - NGINX_WORKER_CONNECTIONS=4096
    deploy:
      resources:
        limits:
          memory: 2G
        reservations:
          memory: 1G

volumes:
  hls_data:
  recordings_data:
```

### Load Balancing

```nginx
# Load balancer configuration
upstream rtmp_backend {
    server rtmp1.revent.social:1935;
    server rtmp2.revent.social:1935;
    server rtmp3.revent.social:1935;
}

server {
    listen 1935;
    proxy_pass rtmp_backend;
}
```

### CDN Integration

```nginx
# Add CDN headers for HLS content
location /hls {
    add_header Cache-Control "public, max-age=10";
    add_header Access-Control-Allow-Origin *;
    add_header Access-Control-Allow-Methods "GET, HEAD";
    
    # CDN optimization
    expires 10s;
}
```

## 🔐 Security

### Stream Key Authentication

```nginx
# Authentication endpoint
location /auth {
    # Validate stream key against database
    proxy_pass http://auth-service/validate;
    proxy_pass_request_body off;
    proxy_set_header Content-Length "";
    proxy_set_header X-Original-URI $request_uri;
}
```

### Rate Limiting

```nginx
# Rate limiting configuration
limit_req_zone $binary_remote_addr zone=streaming:10m rate=5r/s;

location /hls {
    limit_req zone=streaming burst=10 nodelay;
}
```

### SSL/TLS Configuration

```nginx
# HTTPS configuration for HLS
server {
    listen 443 ssl http2;
    server_name streaming.revent.social;
    
    ssl_certificate /etc/ssl/certs/revent.crt;
    ssl_certificate_key /etc/ssl/private/revent.key;
    
    location /hls {
        root /var/www/html;
        add_header Access-Control-Allow-Origin *;
    }
}
```

## 🔍 Troubleshooting

### Common Issues

**RTMP Connection Failed:**
```bash
# Check if port is open
telnet localhost 1935

# Check nginx logs
docker-compose logs nginx-rtmp
```

**HLS Playback Issues:**
```bash
# Verify HLS files are generated
ls -la hls/

# Check HLS playlist
curl http://localhost:8888/hls/stream-key.m3u8
```

**WebRTC Connection Problems:**
```bash
# Check WebRTC logs
docker-compose logs -f nginx-rtmp | grep webrtc

# Verify STUN/TURN configuration
```

### Performance Issues

**High CPU Usage:**
- Reduce transcoding quality settings
- Use hardware acceleration if available
- Optimize nginx worker configuration

**Memory Issues:**
- Limit HLS playlist length
- Reduce buffer sizes
- Monitor for memory leaks

## 🤝 Contributing

### Development Setup

1. Fork the repository
2. Create a feature branch
3. Make your changes to nginx configuration
4. Test with Docker Compose
5. Submit a pull request

### Testing

```bash
# Test RTMP ingestion
ffmpeg -re -i test-video.mp4 -f flv rtmp://localhost:1935/live/test

# Test HLS output
curl -I http://localhost:8888/hls/test.m3u8

# Load testing
wrk -t12 -c400 -d30s http://localhost:8888/hls/test.m3u8
```

---

For more information about the complete Revent platform, see the [main README](../README.md).