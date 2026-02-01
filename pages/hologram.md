# 홀로그램 효과 레퍼런스

## Live Embed
<iframe src="./hologram.html" width="100%" height="360" style="border:1px solid #333;"></iframe>

## GLSL 예제 (Fragment)
```glsl
// Hologram scan + glow (toy)
uniform vec2 iResolution;
uniform float iTime;

float scan(float y){
  return step(0.5, fract(y*120.0 + iTime*0.5));
}

void mainImage(out vec4 fragColor, in vec2 fragCoord){
  vec2 uv = fragCoord / iResolution.xy;
  vec3 col = vec3(0.02,0.06,0.08);

  // glow core
  float glow = exp(-length(uv-0.5)*6.0);
  col += vec3(0.2,0.8,0.9) * glow * 0.4;

  // scanlines
  float s = scan(fragCoord.y);
  col += vec3(0.1,0.6,0.7) * s * 0.25;

  fragColor = vec4(col,1.0);
}
```

## 구현 방법/해설
- **Holo Glow**: 중심부에 청록 발광을 주어 홀로그램 느낌을 강조.
- **Scanlines**: 빠르게 흐르는 줄무늬를 얇게 얹어 디지털 감각 추가.
- **Sprite 사용**: `sample.png`를 텍스처로 사용해 효과 확인 가능.

### 주요 변수
- `scan_freq`: 줄 간격. 높을수록 촘촘.
- `scan_speed`: 흐름 속도.
- `glow_strength`: 중심부 발광 강도.
- `glow_radius`: 발광 범위.

### 튜닝 팁
- 발광이 강하면 **glow_strength**를 낮춰 투명감을 유지.
- 스캔 라인은 `scan_speed`를 빠르게 하면 홀로그램의 깜빡임 느낌이 살아난다.
