# Product Requirements Document (PRD)
# AI Sales Training Platform v2.0 - Final Specification

**Project**: AI Sales Training Platform для Genspark AI Developer  
**Version**: 2.0 (Updated with ElevenLabs Agent Integration)  
**Date**: 2025-08-27  
**Status**: Implementation Complete ✅

---

## 1. 🎯 Executive Summary

### 1.1 Vision Statement
Создать инновационную веб-платформу для тренировки навыков продаж с помощью голосовых AI-помощников, которая позволяет продавцам практиковаться в реальных сценариях, получать детальную аналитику от GPT-4 и улучшать свои результаты через живое взаимодействие с ElevenLabs Conversational AI агентами.

### 1.2 Key Success Metrics
- **Безопасность**: 100% user-provided API keys (никаких hardcoded ключей)
- **Real Data**: Полная замена mock данных на реальные API интеграции
- **Dual Mode**: Поддержка традиционных сценариев + живой диалог с AI агентом
- **GPT-4 Analysis**: Профессиональный анализ разговоров с персонализированными рекомендациями

---

## 2. 🚀 Core Features & Requirements

### 2.1 ✨ ElevenLabs Conversational AI Integration (PRIMARY FEATURE)

#### 2.1.1 Dual-Mode Architecture
**REQUIREMENT**: Платформа должна поддерживать два режима работы с автоматическим переключением:

1. **🤖 Agent Mode** (Приоритетный режим)
   - Использует ElevenLabs Conversational AI SDK
   - Real-time WebSocket communication
   - Живой диалог с настоящим AI агентом
   - Автоматическое получение transcript через ElevenLabs API
   - Динамическая генерация ответов на основе контекста

2. **✅ Traditional Mode** (Fallback режим)
   - Предустановленные 5-этапные сценарии продаж
   - ElevenLabs TTS API для синтеза речи
   - Структурированная оценка по этапам

#### 2.1.2 Technical Implementation
```javascript
// Ключевые компоненты:
- elevenlabs-agent.js - Wrapper для ElevenLabs Conversational AI SDK
- WebSocket-based real-time communication  
- Event handling (connect/disconnect/message/error)
- Conversation transcript retrieval via API
- Integration с secure key management system
```

### 2.2 🔐 Secure API Key Management (CRITICAL REQUIREMENT)

#### 2.2.1 User-Provided Keys Only
**СТРОГО ЗАПРЕЩЕНО**: Любые hardcoded API ключи в коде
**ОБЯЗАТЕЛЬНО**: Все ключи вводятся пользователями через безопасный интерфейс

#### 2.2.2 Required API Keys
1. **ElevenLabs API Key** 
   - Format: `sk_xxxxxxxx...` или `sk-xxxxxxxx...` или UUID
   - Purpose: Traditional Mode TTS generation
   - Status: Optional

2. **🆕 ElevenLabs Agent ID** 
   - Format: `agent_xxxxxxxxxxxxxxxx` или UUID
   - Purpose: Agent Mode conversational AI
   - Status: Optional  
   - UI: Marked with "🆕 NEW" badge

3. **OpenAI API Key**
   - Format: `sk-xxxxxxxxxxxxxxxxxxxxxxxx` (48+ chars)
   - Purpose: GPT-4 conversation analysis
   - Status: Required

#### 2.2.3 Security Features
- **Memory-based encryption** для хранения ключей
- **Auto-expiration** через 30 минут или при закрытии вкладки
- **Format validation** для всех типов ключей
- **Real API testing** для валидации ключей

### 2.3 📊 Real Data Analysis (NO MOCK DATA)

#### 2.3.1 OpenAI GPT-4 Integration
**REQUIREMENT**: Полная замена mock аналитики на реальный анализ GPT-4

```javascript
// Conversation Analysis Pipeline:
1. Retrieve conversation data (traditional responses OR agent transcript)
2. Send to OpenAI GPT-4 with specialized sales analysis prompt
3. Generate detailed scores, recommendations, and insights
4. Display professional analytics with visualizations
```

#### 2.3.2 Data Sources
- **Traditional Mode**: User responses + AI messages from local capture
- **Agent Mode**: Real conversation transcript from ElevenLabs API
- **Unified Analysis**: Consistent GPT-4 analysis regardless of source

### 2.4 🎤 Voice Interaction System

#### 2.4.1 Speech Recognition
- **Web Speech API** для real-time распознавания речи
- **Continuous listening** в режиме разговора
- **Fallback to text input** при недоступности микрофона
- **Multi-language support** (Russian primary)

#### 2.4.2 Voice Generation
- **Agent Mode**: Native ElevenLabs agent voice через WebSocket
- **Traditional Mode**: ElevenLabs TTS API
- **Fallback**: Browser SpeechSynthesis API

---

## 3. 🏗️ Technical Architecture

### 3.1 Core Components

#### 3.1.1 New Files (ElevenLabs Integration)
```
elevenlabs-agent.js     - ElevenLabs Conversational AI wrapper
conversation-analyzer.js - Enhanced GPT-4 analysis (replaces mock)
secure-key-manager.js   - Enhanced key management + Agent ID support
api-keys-setup.js      - Updated UI with Agent ID field
```

#### 3.1.2 Updated Files
```
script.js              - Dual-mode conversation logic
index.html            - ElevenLabs SDK integration
styles.css            - New UI components and badges
README.md             - Complete documentation update
```

### 3.2 Smart Status System

**REQUIREMENT**: Интеллектуальное определение доступных режимов на основе введенных ключей

```javascript
// Status Logic:
🤖 Agent Mode     = Agent ID + OpenAI Key
✅ Traditional    = ElevenLabs API + OpenAI Key  
⚠️ Partial Setup  = Some keys missing
🔑 No Keys        = Demo mode only
```

### 3.3 Progressive Web App (PWA)
- **Service Worker** для offline functionality
- **Responsive design** для всех устройств
- **Installable** через браузер
- **Performance optimized** загрузка

---

## 4. 📋 User Experience Requirements

### 4.1 API Keys Setup Flow

#### 4.1.1 Interface Requirements
```
┌─ API Keys Setup Modal ─┐
│ 🔑 ElevenLabs API Key    │ [Optional]
│ 🆕 ElevenLabs Agent ID  │ [Optional] [NEW badge]
│ 🔑 OpenAI API Key       │ [Required]
│                         │
│ [Skip Demo] [Save Keys] │
└─────────────────────────┘
```

#### 4.1.2 Validation & Testing
- **Real-time format validation** для всех ключей
- **API testing buttons** с live результатами
- **Visual feedback** (success/error states)
- **Help links** для получения ключей

### 4.2 Conversation Experience

#### 4.2.1 Agent Mode Flow
1. **Connection**: "Соединение с AI агентом..."
2. **Ready**: "Агент подключен. Говорите!"
3. **Conversation**: Real-time dialogue с живыми ответами
4. **Analysis**: Transcript retrieval + GPT-4 analysis
5. **Results**: Professional score + recommendations

#### 4.2.2 Traditional Mode Flow
1. **Scenario Start**: Predefined 5-step sales scenario
2. **Step-by-step**: Structured conversation flow
3. **Response Capture**: User speech recognition
4. **AI Responses**: TTS-generated replies
5. **Analysis**: Local data + GPT-4 analysis

### 4.3 Results & Analytics

#### 4.3.1 Scoring System
- **Overall Score**: 0-100 points с визуальной индикацией
- **Detailed Breakdown**: 5 критериев с весами
- **Radar Chart**: Визуализация сильных/слабых сторон
- **Personalized Recommendations**: От GPT-4

#### 4.3.2 Historical Data
- **LocalStorage persistence** результатов
- **Session management** для пользователей
- **Export functionality** для данных

---

## 5. 🔧 Technical Specifications

### 5.1 Frontend Stack
```
HTML5/CSS3/JavaScript (ES6+)
Chart.js для visualizations
Font Awesome icons
Google Fonts (Inter)
ElevenLabs Conversational AI SDK
Web Speech API
Service Workers (PWA)
```

### 5.2 External API Integrations

#### 5.2.1 ElevenLabs APIs
```javascript
// TTS API (Traditional Mode)
POST https://api.elevenlabs.io/v1/text-to-speech/{voice_id}

// Conversational AI SDK (Agent Mode)  
WebSocket connection для real-time communication

// Conversations API (Transcript Retrieval)
GET https://api.elevenlabs.io/v1/convai/conversations/{conversation_id}
```

#### 5.2.2 OpenAI API
```javascript
// GPT-4 Analysis
POST https://api.openai.com/v1/chat/completions
Model: gpt-4-turbo
Purpose: Sales conversation analysis with specialized prompts
```

### 5.3 Security & Privacy

#### 5.3.1 Data Protection
- **No server storage** API ключей
- **Memory-only encryption** с auto-expiration
- **No tracking/analytics** third-party services
- **HTTPS required** для production

#### 5.3.2 CORS & Security Headers
- **Content Security Policy** настроенная для external APIs
- **CORS handling** для ElevenLabs/OpenAI endpoints
- **Input sanitization** для user data

---

## 6. 📱 User Interface Specifications

### 6.1 Color Scheme & Branding
```css
Primary: #4f46e5 (Indigo)
Secondary: #059669 (Green)  
Success: #10b981 (Emerald)
Warning: #f59e0b (Amber)
Error: #ef4444 (Red)
```

### 6.2 Typography
```css
Font Family: 'Inter' (Google Fonts)
Weights: 300, 400, 500, 600, 700
Responsive sizing: rem-based
```

### 6.3 Component Library

#### 6.3.1 New UI Components
```css
.new-feature-badge     - Animated green badge для Agent ID
.status-indicator      - Smart API keys status
.keys-partial          - Partial setup warning state
.api-keys-modal        - Enhanced setup interface
```

#### 6.3.2 Responsive Breakpoints
```css
Mobile: 320px - 768px
Tablet: 768px - 1024px  
Desktop: 1024px+
```

---

## 7. 🧪 Testing & Quality Assurance

### 7.1 Test Coverage Requirements

#### 7.1.1 API Integration Tests
- **ElevenLabs TTS API** connection testing
- **ElevenLabs Agent ID** format validation
- **OpenAI GPT-4** analysis pipeline testing
- **Error handling** для network failures

#### 7.1.2 User Interface Tests
- **API Keys setup** flow testing
- **Conversation modes** switching testing
- **Status indicators** logic verification
- **Responsive design** на всех устройствах

#### 7.1.3 Security Tests
- **Key encryption/decryption** cycles
- **Memory cleanup** verification
- **Format validation** bypass attempts
- **XSS/injection** prevention

### 7.2 Performance Requirements
- **Page load time**: < 3 seconds
- **API response time**: < 2 seconds
- **Memory usage**: < 100MB
- **Mobile performance**: 60fps animations

---

## 8. 📚 Documentation Requirements

### 8.1 User Documentation
- **README.md**: Complete setup and usage guide
- **API-KEYS-GUIDE.md**: Step-by-step key obtaining instructions
- **SECURITY.md**: Security best practices and warnings

### 8.2 Developer Documentation
- **Code comments**: Comprehensive inline documentation
- **Architecture diagrams**: System design visualization
- **API integration examples**: Code snippets for each service

### 8.3 Compliance Documentation
- **Privacy Policy**: Data handling transparency
- **Terms of Service**: Usage guidelines
- **API Usage Guidelines**: Third-party service compliance

---

## 9. 🚀 Deployment & Operations

### 9.1 Deployment Strategy
- **Static hosting**: Netlify, Vercel, GitHub Pages compatible
- **PWA publishing**: Browser installation support
- **CDN optimization**: Global content delivery
- **Version control**: Git-based deployment workflow

### 9.2 Monitoring & Analytics
- **Error tracking**: Console error monitoring
- **Performance metrics**: Page load analytics
- **User feedback**: Built-in feedback collection
- **Usage statistics**: Anonymous usage patterns

---

## 10. 🎯 Success Criteria & Acceptance

### 10.1 Functional Requirements ✅
- [x] **ElevenLabs Agent ID field** added to API setup
- [x] **Dual-mode conversation** system implemented
- [x] **Real GPT-4 analysis** replacing all mock data
- [x] **Secure key management** with user-provided keys only
- [x] **WebSocket communication** для agent interactions
- [x] **Transcript retrieval** from ElevenLabs API

### 10.2 Non-Functional Requirements ✅
- [x] **No hardcoded API keys** in source code
- [x] **Mobile-responsive** design
- [x] **PWA compliance** with service worker
- [x] **Accessibility** standards (WCAG 2.1)
- [x] **Performance optimization** < 3s load time
- [x] **Cross-browser compatibility** (Chrome, Firefox, Safari, Edge)

### 10.3 User Experience Requirements ✅  
- [x] **Intuitive API setup** с smart status indicators
- [x] **Seamless mode switching** based on available keys
- [x] **Professional analytics** с visualization
- [x] **Real-time feedback** во время разговора
- [x] **Helpful error messages** и recovery suggestions

---

## 11. 📋 Project Deliverables

### 11.1 Core Application Files
```
index.html                 - Main application page
script.js                 - Enhanced dual-mode conversation logic  
styles.css                - Complete UI styling with new components
elevenlabs-agent.js       - ElevenLabs Conversational AI wrapper
conversation-analyzer.js  - Real GPT-4 analysis implementation
secure-key-manager.js     - Enhanced secure key management
api-keys-setup.js         - Updated API setup with Agent ID field
```

### 11.2 Configuration & Assets
```
config.js                 - Application configuration
utils.js                  - Utility functions
sw.js                     - Service worker for PWA
assets/                   - Images, icons, audio files
```

### 11.3 Documentation Files
```
README.md                 - Complete project documentation
PRD-FINAL.md             - This technical specification
SECURITY.md              - Security guidelines and warnings
API-KEYS-GUIDE.md        - API key obtaining instructions
AGENT-INTEGRATION-SUMMARY.md - Integration completion summary
```

### 11.4 Testing Files  
```
test-agent-setup.html     - Agent ID setup interface testing
test-key-validation.html  - API key validation testing
examples.html            - Usage examples and demos
```

---

## 12. 🎉 Implementation Status

### 12.1 Completed Features ✅
- **ElevenLabs Conversational AI SDK integration**
- **Agent ID field in API setup with NEW badge**
- **Dual-mode conversation system (Agent + Traditional)**
- **Real GPT-4 conversation analysis**
- **Smart API keys status indicators**
- **Secure user-provided key management**
- **WebSocket real-time communication**
- **Conversation transcript retrieval**
- **Enhanced UI with new components**
- **Complete documentation update**

### 12.2 Technical Validation ✅
- **No hardcoded API keys** in source code
- **All mock data replaced** with real API integrations  
- **Security best practices** implemented
- **Cross-browser compatibility** verified
- **Mobile responsiveness** confirmed
- **PWA functionality** working

### 12.3 User Experience Validation ✅
- **API setup flow** intuitive and complete
- **Mode switching** seamless and automatic
- **Error handling** comprehensive and helpful
- **Analytics presentation** professional and actionable
- **Performance metrics** meeting requirements

---

## 13. 🔮 Future Enhancements (Out of Scope)

### 13.1 Advanced Features
- **Multi-language agent support**
- **Custom agent training**
- **Team collaboration features**
- **Advanced analytics dashboard**
- **Integration with CRM systems**

### 13.2 Enterprise Features
- **SSO authentication**
- **Role-based access control**
- **Audit logging**
- **Data export/import**
- **White-label customization**

---

**📝 Document Version**: 2.0 Final  
**👤 Product Owner**: Genspark AI Developer  
**🏗️ Implementation**: Complete ✅  
**📅 Last Updated**: 2025-08-27

---

*This PRD represents the complete and final specification for the AI Sales Training Platform with full ElevenLabs Agent integration, secure API key management, and real data analysis capabilities.*
