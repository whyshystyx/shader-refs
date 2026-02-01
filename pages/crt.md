# CRT 효과 레퍼런스

## Live Embed
<iframe src="./crt.html" width="100%" height="360" style="border:1px solid #333;"></iframe>

## GLSL 예제 (Fragment)
```glsl
// CRT scanlines + vignette (toy)
uniform vec2 iResolution;
uniform float iTime;

float scanline(float y){
  return 0.5 + 0.5*sin(y*3.14159*2.0);
}

void mainImage(out vec4 fragColor, in vec2 fragCoord){
  vec2 uv = fragCoord / iResolution.xy;
  // base color
  vec3 col = vec3(0.05,0.08,0.12);

  // scanlines
  float s = scanline(fragCoord.y*0.5);
  col *= mix(0.75, 1.0, s);

  // vignette
  vec2 p = uv*2.0-1.0;
  float v = 1.0 - smoothstep(0.3, 1.2, dot(p,p));
  col *= v;

  fragColor = vec4(col,1.0);
}
```

## 구현 방법/해설
- **Scanlines**: 화면 Y좌표에 사인 함수를 적용해 줄무늬를 만든다.
- **Vignette**: 화면 중심에서 멀어질수록 어두워지도록 `dot(p,p)` 기반 감쇠.
- **Sprite 사용**: `sample.png`를 텍스처로 적용해 왜곡/노이즈 확인 가능.

### 주요 변수
- `scanline_freq`: 줄 간격. 높을수록 촘촘.
- `scanline_strength`: 줄무늬 대비.
- `vignette_radius`: 가장자리 어둠 범위.
- `vignette_strength`: 가장자리 감쇠 강도.

### 튜닝 팁
- 줄무늬가 과하면 **scanline_strength**를 낮추고 **freq**를 낮춘다.
- CRT 느낌을 더 내려면 **색 채널 분리**(RGB offset)와 **노이즈**를 추가.
