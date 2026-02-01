# 마우스 틸팅 효과 레퍼런스

## Live Embed
<iframe src="./tilt.html" width="100%" height="300" style="border:1px solid #333;"></iframe>

## GLSL 예제 (Vertex)
```glsl
// Simple tilt by cursor (vertex)
attribute vec3 position;
uniform mat4 projectionMatrix, modelViewMatrix;
uniform vec2 uMouse; // 0..1

void main(){
  float rx = (0.5 - uMouse.y) * 0.4; // tilt X
  float ry = (uMouse.x - 0.5) * 0.5; // tilt Y

  mat4 rotX = mat4(1,0,0,0, 0,cos(rx),-sin(rx),0, 0,sin(rx),cos(rx),0, 0,0,0,1);
  mat4 rotY = mat4(cos(ry),0,sin(ry),0, 0,1,0,0, -sin(ry),0,cos(ry),0, 0,0,0,1);

  gl_Position = projectionMatrix * modelViewMatrix * rotY * rotX * vec4(position,1.0);
}
```

## 구현 방법/해설
- 마우스 좌표를 **0..1 정규화**한 뒤, X/Y 회전 각도로 변환.
- UI 카드, 아이템, 캐릭터 초상 등 **입체감 강조 요소**에 효과적.

### 주요 변수
- `tilt_strength_x/y`: 회전 강도.
- `smooth`: 마우스 추적 보간 속도.
- `depth`: 입체감을 주는 z 오프셋.

### 튜닝 팁
- 모바일에서는 **자이로** 입력으로 대체 가능.
- 과도한 회전은 피하고 **5~12도** 범위가 안정적.
