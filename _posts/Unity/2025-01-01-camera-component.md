---
title: "Camera"
categories: Unity
tag: [컴포넌트]
---




<span style="color:gray">unity version 2022.3.7f1</span>




# Camera
{: .notice}




## Clear Flags
{: .notice--primary}

카메라의 빈 부분을 어떻게 표시할지 정할 수 있습니다.

<span class="ul-1">
<span class="color-keyword">Skybox</span> : 카메라가 새로운 프레임을 렌더링할 때 이전 프레임을 지우고 스카이박스 위에 새 이미지를 표시합니다.
</span>

<span class="ul-1">
<span class="color-keyword">Solid Color</span> : 카메라가 새로운 프레임을 렌더링할 때 이전 프레임을 지우고 단색 위에 새 이미지를 표시합니다.
</span>

<span class="ul-1">
<span class="color-keyword">Depth Only</span> : 카메라가 새로운 프레임을 렌더링할 때 이전 프레임의 깊이 정보를 지우고 새 프레임을 위에 표시합니다. <span class="color-comment"><span class="color-class">두 개 이상의 카메라</span>를 사용해 카메라를 겹쳐 놓을 때 사용하는데, 예를 들어 FPS 게임에서 벽에 매우 근접했을 때 총이 벽을 관통해 잘리는 것을 피하고 싶을 때 첫 번째 카메라(Clear Flags-Skybox)는 환경을 표시하고 두 번째 카메라(Depth Only)는 총을 표시. 카메라 순서는 <span class="color-class">Depth</span> 매개변수를 사용하여 선택되며 Depth가 더 큰 카메라가 다른 카메라 위에 표시되며 게임 객체의 선택적 표시는 <span class="color-class">Culling Mask</span>를 사용합니다.</span>
</span>

<span class="ul-1">
<span class="color-keyword">Don't clear</span> : 카메라가 새로운 프레임을 렌더링할 때 이전 프레임을 지우지 않고 위에 새 이미지를 표시합니다.
</span>




## Culling Mask
{: .notice--primary}

해당 카메라가 레이어를 기준으로 지정된 객체만 그리도록 합니다.




## Projection
{: .notice--primary}

<span class="ul-1">
<span class="color-keyword">Perspective</span> : Z축, 깊이감을 이용해 카메라에 원근법을 적용합니다.(3D)
</span>

<span class="ul-2">
<span class="color-variable">FOV Axis</span> : Field of View에서 기준으로 사용할 축을 정합니다. Vertical(수직), Horizontal(수평)
</span>

<span class="ul-2">
<span class="color-variable">Field of View</span> : FOV Axis에서 정한 축으로 시야각을 조절합니다.
</span>

<span class="ul-2">
<span class="color-variable">Physical Camera</span> : Enable로 둘 경우 속성 영역이 나타나는데, 실제 카메라 렌즈를 모방합니다.
</span>

<span class="ul-1">
<span class="color-keyword">Orthographic</span> : Z축, 깊이감과 상관없이 오브젝트들의 크기가 일정합니다.(2D)
</span>




## Clipping Planes
{: .notice--primary}

카메라가 Near(starts)부터 Far(stops)까지의 영역만 렌더링합니다.




## Viewport Rect
{: .notice--primary}

X, Y, W(width), H(height)옵션을 통해 해당 카메라가 화면에서 보여질 영역을 조절합니다.




## Depth
{: .notice--primary}

카메라 렌더링 우선순위로 숫자가 높을수록 먼저 렌더링됩니다.




## Rendering Path
{: .notice--primary}

<span class="color-comment">(실시간 조명에 의한 최적의 성능을 위해)</span>카메라에 사용될 렌더링 메서드를 정합니다.

<span class="ul-1">
<span class="color-keyword">Use Graphics Settings</span> : <span class="color-control">Edit</span>-><span class="color-control">ProjectSettings</span>-><span class="color-control">Grahpics</span> 설정을 사용합니다.
</span>

<span class="ul-1">
<span class="color-keyword">Forward</span> : 조명이 있는 객체는 별도의 패스로 렌더링합니다. 렌더링이 매우 빨라 하드웨어 요구 사항을 줄이는 데 도움이 되지만 조명당 렌더링 비용이 있어 씬에 조명이 많을수록 렌더링에 많은 비용이 소모됩니다. 렌더링 경로가 조명당 해당 객체를 여러 번 렌더링하기 때문입니다.
</span>

<span class="ul-1">
<span class="color-keyword">Deferred</span> : 빛 정보의 음영 처리와 블렌딩을 화면을 처음 통과한 후까지 미룹니다. 렌더링 비용이 조명 자체의 수가 아닌 조명이 비추는 픽셀 수에 비례해 조명이 많은 씬의 경우 렌더링 비용을 상당히 줄여주지만 일반적으로 더 강력한 하드웨어가 필요하고 특정 모바일 기기에서 지원되지 않습니다.
</span>

<span class="ul-1">
<span class="color-keyword">Legacy Vertex Lit</span> : 각 객체를 한 번의 패스로 렌더링하며 각 정점에 대해 모든 조명의 조명이 계산됩니다. 가장 빠른 렌더링 경로이며 가장 광범위한 하드웨어 지원을 제공합니다. 모든 조명은 정점 수준에서 계산되어 대부분의 픽셀별 효과를 지원하지 않습니다. 그림자, 노멀 매핑, 라이트 쿠키, 매우 자세한 반사 하이라이트가 지원되지 않습니다.
</span>




## Target Texture
{: .notice--primary}

텍스쳐를 카메라가 비추는 영역으로 만들 수 있습니다.<span class="color-comment">(거울, 망원경, 사진 등)</span>




## Occlusion Culling
{: .notice--primary}

카메라를 기준으로 가까운 오브젝트에 의해 가려져 보이지 않는 오브젝트는 렌더링 하지 않는 기능입니다.

<a href="https://CozyCozy4Tato.github.io/unity/occlusion-culling-frustum-culling/" target="_blank" class="color-function">오클루전 컬링 페이지</a>




## HDR
{: .notice--primary}

HDR(High Dynamic Range)기능을 Use Graphics Settings(<span class="color-control">Edit</span>→<span class="color-control">ProjectSettings</span>→<span class="color-control">Grahpics</span>) 또는 off 할 수 있습니다.




## MSAA
{: .notice--primary}

MSAA(Multi Sampling Anti-Aliasing)기능을 Use Graphics Settings(<span class="color-control">Edit</span>→<span class="color-control">ProjectSettings</span>→<span class="color-control">Grahpics</span>) 또는 off 할 수 있습니다.




## Allow Dynamic Resolution
{: .notice--primary}

개별 렌더 타겟을 동적으로 스케일링하여 GPU의 부하를 줄여주는 카메라 설정입니다. 애플리케이션의 프레임 속도가 감소하는 경우에는 일관된 프레임 속도를 유지하기 위해 점진적으로 해상도를 스케일다운할 수 있습니다. 성능 데이터가 애플리케이션의 GPU 바운드로 인해 프레임 속도가 떨어진다고 암시하는 경우 Unity는 이 스케일링 방식을 트리거합니다.




## Target Display
{: .notice--primary}

다중 모니터를 사용할 경우 표시될 모니터를 지정합니다.