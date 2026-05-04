# 💻 Developer — business와 designer가 확정한 스펙을 바탕으로 웹사이트의 초기 기술 스택(Tech Stack)을 결정하고, 기본적인 프로젝트 아키텍처를 설정해 주세요. 우선은 와이어프레임의 뼈대(Front-end Shell)부터 구축하여, 이후 디자인 요소들이 결합될 수 있는 환경을 준비하는 데 집중해야 합니다.

💻 Developer: 작업 시작합니다.

## 🏗️ 웹사이트 초기 기술 스택(Tech Stack) 및 아키텍처 설계

CEO님의 지시사항과 Business, Designer 팀이 확정한 KPI 및 디자인 시스템을 완벽하게 구현하기 위한 가장 효율적이고 확장성 높은 스택을 선정하고, 프론트엔드 쉘터(Shell)를 구축했습니다. 이 아키텍처는 리액트 컴포넌트 기반으로 되어 있어 향후 기능 추가나 A/B 테스트가 용이합니다.

### 1. 기술 스택 결정 및 근거 (Tech Stack Rationale)

| 영역 | 선택 기술 | 선정 이유 | 비고 |
| :--- | :--- | :--- | :--- |
| **프레임워크** | **Next.js (React 기반)** | ✨ **SEO 최적화:** 리드 확보(Lead Generation)가 목표이므로 검색 엔진 노출이 필수입니다. Next.js는 SSR/SSG를 기본 지원하여 SEO에 가장 유리합니다. <br> ✨ **풀스택 통합:** 프론트엔드와 API 라우팅을 하나의 프로젝트에서 관리할 수 있어 개발 속도가 빠르고 아키텍처가 단순합니다. | MVP 구축에 최적화됨 |
| **스타일링** | **Tailwind CSS + CSS Variables** | 🎨 **디자인 시스템 준수:** Designer가 정의한 모든 색상 코드를 CSS 변수로 선언하여, 어떤 컴포넌트에서도 일관된 디자인 토큰을 사용할 수 있습니다. <br> ✨ **속도 및 유지보수성:** 클래스 기반으로 빠르게 레이아웃을 구성할 수 있어 개발 속도가 빠릅니다. | 재사용 가능한 UI 키트에 최적 |
| **상태 관리** | (초기 단계) Context API / Zustand | 복잡한 전역 상태가 발생하기 전까지는 Next.js의 기본 훅(Hooks)만 사용합니다. 필요 시 경량화된 라이브러리를 도입하여 오버헤드를 최소화합니다. | - |

### 2. 프로젝트 아키텍처 (Folder Structure & Setup)

전체적인 폴더 구조와 역할을 정의했습니다.

```
/project-root
├── app/                 # Next.js 라우팅 및 페이지 컴포넌트가 위치하는 최상위 폴더
│   ├── layout.tsx       # 모든 페이지에 적용되는 기본 레이아웃 (헤더, 푸터 등)
│   └── page.tsx         # 메인 랜딩 페이지 (/)의 쉘 구조를 담는 핵심 파일
├── components/          # 재사용 가능한 UI 컴포넌트 저장소 (가장 중요!)
│   ├── ui/              # 기본 HTML 요소 기반 컴포넌트 (Button, Card, Input)
│   ├── layout/          # 레이아웃 관련 컴포넌트 (Header, Footer, SectionWrapper)
│   └── sections/        # 페이지의 논리적 섹션 단위 (HeroSection, ProofSection 등)
├── styles/              # 전역 스타일 및 CSS 변수 정의 폴더
│   └── variables.css    # Tailwind 커스터마이징을 위한 CSS Variables 선언부
└── lib/                 # API 호출 로직, 유틸리티 함수 등 비즈니스 로직 분리
```

### 3. 실행 가능한 초기 설정 코드 (Executable Code)

#### Step 1: `styles/variables.css` 파일 생성 및 색상 변수 주입
Designer가 정의한 모든 핵심 컬러를 시스템 레벨에서 사용할 수 있도록 CSS 변수로 선언합니다. 이 파일을 Next.js 전역 스타일에 임포트할 것입니다.

```css
/* styles/variables.css */

:root {
    /* Primary Colors (신뢰성) */
    --color-navy-deep: #0D2B58; 
    --color-primary-accent: #00A896; /* CTA 핵심 색상 */

    /* Secondary & Neutral Colors (가독성) */
    --color-slate-light: #EAEFF4;
    --color-charcoal-text: #333333;
    --color-white: #FFFFFF;
}
```

#### Step 2: `components/layout/Header.tsx` (헤더 컴포넌트)
모든 페이지의 최상단에 붙는 구조적 요소입니다. (Placeholder로 네비게이션만 구현)

```tsx
// components/layout/Header.tsx
import React from 'react';

export const Header: React.FC = () => {
  return (
    <header className="bg-[var(--color-navy-deep)] text-white shadow-lg p-4">
      <div className="max-w-6xl mx-auto flex justify-between items-center">
        {/* 로고 영역 */}
        <h1 className="text-2xl font-bold tracking-tight">JAY CORP.</h1>
        {/* 네비게이션 (MVP 단계에서는 최소화) */}
        <nav>
          <a href="#service" className="mx-4 hover:text-[var(--color-primary-accent)] transition duration-150">서비스</a>
          <a href="#proof" className="mx-4 hover:text-[var(--color-primary-accent)] transition duration-150">성공 사례</a>
          {/* CTA 버튼을 헤더에 배치하여 접근성을 높임 */}
          <button className="bg-[var(--color-primary-accent)] text-white px-6 py-2 rounded-lg hover:bg-opacity-90 transition duration-150 ml-4">
            무료 문의 신청
          </button>
        </nav>
      </div>
    </header>
  );
};
```

#### Step 3: `app/page.tsx` (랜딩 페이지 쉘)
실제 랜딩 페이지의 골격을 만듭니다. 모든 섹션은 구조적 마크업만 갖추고, 실제 콘텐츠(헤드라인, 문단 등)는 나중에 채워 넣을 준비가 되어 있습니다.

```tsx
// app/page.tsx (메인 랜딩 페이지 쉘터)
import React from 'react';
import { Header } from '@/components/layout/Header';

export default function HomePage() {
  return (
    <div className="min-h-screen bg-white">
      {/* 1. Global Layout */}
      <Header />
      
      <main>
        {/* 2. Hero Section: 가장 중요한 가치 제안 섹션 */}
        <section id="hero" className="bg-[var(--color-slate-light)] py-24 text-center">
          <div className="max-w-4xl mx-auto px-6">
            {/* H1 Placeholder - 강력한 Pain Point + Benefit 중심 메시지 */}
            <h1 className="text-5xl font-extrabold text-[var(--color-navy-deep)] mb-6 leading-tight">
              단순 자동화를 넘어, <span className="text-[var(--color-primary-accent)]">지능형 비즈니스</span>로 성장하세요.
            </h1>
            {/* Subtitle Placeholder */}
            <p className="text-xl text-gray-600 mb-10 max-w-2xl mx-auto">
              복잡한 수작업과 시간 낭비는 이제 그만. JAY CORP가 검증된 AI 기반 자동화 솔루션을 제공합니다.
            </p>
            {/* CTA Button Placeholder */}
            <button className="bg-[var(--color-primary-accent)] text-white text-lg px-10 py-4 rounded-xl shadow-xl hover:opacity-90 transition duration-200">
              ✅ 무료 컨설팅 및 진단 신청하기
            </button>
          </div>
        </section>

        {/* 3. Service Overview Section (서비스 소개 - 구조적 뼈대) */}
        <section id="service" className="py-20 px-6 max-w-6xl mx-auto">
          <h2 className="text-4xl font-bold text-[var(--color-navy-deep)] mb-12 text-center">우리가 해결해 드릴 핵심 문제</h2>
          {/* 3~4단계 프로세스 Placeholder (카드 컴포넌트가 들어갈 자리) */}
          <div className="grid md:grid-cols-3 gap-8 text-center">
            {[1, 2, 3].map(step => (
              <div key={step} className="p-6 bg-[var(--color-slate-light)] rounded-xl shadow hover:shadow-lg transition duration-300 border-t-4 border-[var(--color-primary-accent)]">
                <h3 className="text-2xl font-bold text-[var(--color-navy-deep)] mb-3">{`${step}단계`}</h3>
                <p className="text-lg text-gray-700">프로세스의 핵심 내용을 구조화하여 입력할 자리입니다. (예: 진단 -> 설계 -> 구축)</p>
              </div>
            ))}
          </div>
        </section>

        {/* 4. Proof/Testimonials Section (신뢰도 구축 - Placeholder) */}
        <section id="proof" className="py-20 bg-[var(--color-slate-light)] px-6 max-w-6xl mx-auto">
          <h2 className="text-4xl font-bold text-[var(--color-navy-deep)] mb-12 text-center">이미 검증된 성공 사례</h2>
          {/* 실제 고객 후기 카드를 배치할 자리 */}
          <div className="space-y-8">
            <div className="p-8 bg-white rounded-xl shadow border-l-4 border-[var(--color-primary-accent)]">
              <p className="italic text-gray-700">"자동화 도입 후 수작업 시간이 50% 이상 절감되었고, 데이터 파이프라인 안정성이 확보되어 운영 효율성이 극대화되었습니다."</p>
              <p className="mt-4 font-semibold text-[var(--color-navy-deep)]">- A사 대표이사 (IT 분야)</p>
            </div>
          </div>
        </section>

        {/* 5. Final CTA / Form Section - 리드 확보의 핵심 */}
        <section id="contact" className="py-20 px-6 max-w-xl mx-auto text-center">
           <h2 className="text-4xl font-bold text-[var(--color-navy-deep)] mb-8">가장 빠르고 확실한 솔루션, 지금 문의하세요.</h2>
           {/* TODO: 여기에 실제 리드 필터링 폼 컴포넌트(Form)가 들어갈 자리입니다. */}
           <div className="bg-white p-10 rounded-xl shadow-2xl border border-[var(--color-primary-accent)]">
             <p className="text-lg mb-6 text-gray-700">무료 컨설팅을 통해 귀사의 현재 비즈니스 규모와 필요한 자동화 니즈를 진단받으세요.</p>
             {/* 실제 Form 컴포넌트가 연결될 예정 */}
             <button className="bg-[var(--color-primary-accent)] text-white text-xl px-12 py-4 rounded-full shadow-lg hover:scale-105 transition duration-300">
               상세 문의 양식 작성 (클릭)
             </button>
           </div>
        </section>
      </main>

      {/* 6. Footer */}
      <footer className="bg-[var(--color-navy-deep)] text-white p-8 mt-12">
        <div className="max-w-6xl mx-auto text-center text-sm">
          © {new Date().getFullYear()} JAY CORP. All rights reserved. | 개발: Lead Engineer Agent
        </div>
      </footer>
    </div>
  );
}
```

📊 평가: 완료 — 초기 기술 스택, 아키텍처 정의 및 핵심 컴포넌트 쉘터까지 완성되어 다음 단계 진행이 가능함.
📝 다음 단계: 리드 확보 양식(Form) 컴포넌트를 구축하고, 이를 서버 측 API 라우팅과 연결하여 실제 데이터 수집 기능을 구현해야 함.
