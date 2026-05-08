import React, { useState, useEffect } from 'react';
import { motion, AnimatePresence } from 'framer-motion';
import { 
  Bird, Apple, Citron, Tomato, Cherry, Star, 
  BookOpen, X, Menu, Moon, Sun, MousePointer2 
} from 'lucide-react';

// --- 模拟配置数据 ---
const SEED_CONFIG = {
  about: { en: "About", zh: "关于", color: "bg-blue-50", icon: <Bird />, content: "在数字土壤中播种的梦想家。专注于探索、生长与安静叙事。" },
  skills: { en: "Skills", zh: "技能", color: "bg-red-50", icon: <Apple />, content: "React, Three.js, Tailwind, Interaction Design, Storytelling." },
  experience: { en: "Experience", zh: "历程", color: "bg-yellow-50", icon: <Citron />, content: "2024 - 自由设计师 | 2022 - 交互实验室成员" },
  projects: { en: "Projects", zh: "项目", color: "bg-orange-50", icon: <Tomato />, content: "《种子计划》, 《漂浮城市》, 《微光》" },
  thoughts: { en: "Thoughts", zh: "随笔", color: "bg-pink-50", icon: <Cherry />, content: "万物皆有裂痕，那是光照进来的地方。" },
  awards: { en: "Awards", zh: "荣誉", color: "bg-purple-50", icon: <Star />, content: "Awwwards SOTD (Nominee), Red Dot Design Award 2023." },
};

export default function SeedProject() {
  const [loading, setLoading] = useState(true);
  const [starsCollected, setStarsCollected] = useState(0);
  const [activeSection, setActiveSection] = useState(null);
  const [isDarkMode, setIsDarkMode] = useState(false);

  // 模拟加载逻辑
  useEffect(() => {
    if (starsCollected >= 5) {
      setTimeout(() => setLoading(false), 500);
    }
  }, [starsCollected]);

  // --- 1. 像素风加载页面 ---
  if (loading) {
    return (
      <div className="fixed inset-0 bg-[#FDFBF7] flex flex-col items-center justify-center z-50 font-mono">
        <div className="relative w-64 h-32 border-b-2 border-gray-200 overflow-hidden">
          {/* 模拟小狗跑动 */}
          <motion.div 
            animate={{ x: [0, 200, 0] }} 
            transition={{ duration: 4, repeat: Infinity, ease: "linear" }}
            className="absolute bottom-0 text-3xl"
          >
            🐕
          </motion.div>
          {/* 交互星星 */}
          {[...Array(5)].map((_, i) => (
            <motion.button
              key={i}
              whileTap={{ scale: 0.5 }}
              onClick={() => setStarsCollected(s => s + 1)}
              className="absolute cursor-pointer text-yellow-400"
              style={{ left: `${20 + i * 40}px`, top: `${Math.random() * 40}px` }}
            >
              {starsCollected > i ? '✨' : '⭐'}
            </motion.button>
          ))}
        </div>
        <p className="mt-8 text-gray-400 animate-pulse uppercase tracking-widest text-xs">
          Collecting Stars to Start ({starsCollected}/5)
        </p>
      </div>
    );
  }

  return (
    <div className={`min-h-screen transition-colors duration-1000 ${isDarkMode ? 'bg-[#1a1c1e] text-gray-200' : 'bg-[#FDFBF7] text-gray-800'} font-sans overflow-hidden`}>
      
      {/* --- 2. 导航 Fallback --- */}
      <nav className="fixed top-8 right-8 z-40 flex items-center gap-6 text-sm tracking-tighter">
        <button onClick={() => setIsDarkMode(!isDarkMode)} className="p-2 hover:bg-black/5 rounded-full transition-colors">
          {isDarkMode ? <Sun size={18} /> : <Moon size={18} />}
        </button>
        <div className="hidden md:flex gap-6 uppercase opacity-50">
          <span>About</span>
          <span>Work</span>
          <span>Contact</span>
        </div>
        <Menu size={20} className="cursor-pointer" />
      </nav>

      {/* --- 3. 主场景 (3D 模拟) --- */}
      <main className="relative h-screen flex items-center justify-center">
        {/* 背景装饰 */}
        <div className="absolute inset-0 pointer-events-none">
          <div className={`absolute top-1/4 left-1/4 w-64 h-64 rounded-full blur-[120px] ${isDarkMode ? 'bg-blue-900/20' : 'bg-orange-100'}`} />
          <div className="absolute bottom-10 w-full text-center opacity-20 uppercase text-[15vw] font-bold select-none tracking-tighter">
            The Seed
          </div>
        </div>

        {/* 模拟 3D 树结构 */}
        <motion.div 
          initial={{ opacity: 0, scale: 0.8 }}
          animate={{ opacity: 1, scale: 1 }}
          className="relative w-[400px] h-[500px] flex items-center justify-center"
        >
          {/* 树干 */}
          <div className={`w-6 h-64 ${isDarkMode ? 'bg-gray-800' : 'bg-[#D2B48C]'} rounded-t-full relative`}>
            {/* 树冠模拟 */}
            <div className={`absolute -top-32 -left-32 w-72 h-72 rounded-full blur-3xl opacity-30 ${isDarkMode ? 'bg-green-900' : 'bg-green-200'}`} />
            
            {/* 交互果实节点 */}
            <FruitNode id="about" pos="top-[-20px] left-[-40px]" data={SEED_CONFIG.about} onClick={setActiveSection} />
            <FruitNode id="skills" pos="top-[40px] right-[-60px]" data={SEED_CONFIG.skills} onClick={setActiveSection} />
            <FruitNode id="experience" pos="top-[100px] left-[-80px]" data={SEED_CONFIG.experience} onClick={setActiveSection} />
            <FruitNode id="projects" pos="top-[160px] right-[-30px]" data={SEED_CONFIG.projects} onClick={setActiveSection} />
            <FruitNode id="thoughts" pos="top-[200px] left-[20px]" data={SEED_CONFIG.thoughts} onClick={setActiveSection} />
            <FruitNode id="awards" pos="top-[-60px] right-[10px]" data={SEED_CONFIG.awards} onClick={setActiveSection} />
          </div>
        </motion.div>

        {/* 底部文案 */}
        <div className="absolute bottom-12 left-12">
          <h1 className="text-2xl font-light tracking-widest uppercase">The Seed Project</h1>
          <p className="text-gray-400 text-sm">种子计划 — 探索、生长与安静叙事</p>
        </div>
      </main>

      {/* --- 4. 笔记本 UI 展开层 --- */}
      <AnimatePresence>
        {activeSection && (
          <motion.div 
            initial={{ opacity: 0 }}
            animate={{ opacity: 1 }}
            exit={{ opacity: 0 }}
            className="fixed inset-0 z-50 flex items-center justify-center bg-black/20 backdrop-blur-sm p-4"
          >
            <motion.div 
              initial={{ y: 50, scale: 0.9 }}
              animate={{ y: 0, scale: 1 }}
              exit={{ y: 50, scale: 0.9 }}
              className={`relative w-full max-w-xl h-[600px] ${isDarkMode ? 'bg-[#25282c] border-gray-700' : 'bg-white'} shadow-2xl rounded-sm p-12 overflow-y-auto border-l-[12px] border-[#e5e7eb]`}
            >
              <button 
                onClick={() => setActiveSection(null)}
                className="absolute top-8 right-8 p-2 hover:bg-gray-100 rounded-full transition-colors"
              >
                <X size={20} />
              </button>

              <header className="mb-12">
                <span className="text-xs uppercase tracking-[0.3em] text-gray-400 block mb-2">{SEED_CONFIG[activeSection].en}</span>
                <h2 className="text-4xl font-serif">{SEED_CONFIG[activeSection].zh}</h2>
              </header>

              <div className="prose leading-loose text-lg font-light">
                {SEED_CONFIG[activeSection].content}
                <div className="mt-12 h-40 w-full bg-gray-50 rounded animate-pulse" />
              </div>

              <footer className="mt-20 pt-8 border-t border-gray-100 flex justify-between text-xs text-gray-400 italic">
                <span>The Seed Project. No.012</span>
                <span>2026.05.08</span>
              </footer>
            </motion.div>
          </motion.div>
        )}
      </AnimatePresence>

      {/* 背景微粒 */}
      <div className="fixed inset-0 pointer-events-none opacity-30">
        {[...Array(20)].map((_, i) => (
          <motion.div
            key={i}
            animate={{ 
              y: [0, -100], 
              opacity: [0, 1, 0],
              x: Math.sin(i) * 50
            }}
            transition={{ duration: 5 + Math.random() * 5, repeat: Infinity, delay: i }}
            className="absolute w-1 h-1 bg-white rounded-full"
            style={{ left: `${Math.random() * 100}%`, top: `${Math.random() * 100}%` }}
          />
        ))}
      </div>
    </div>
  );
}

// --- 5. 辅助组件：果实节点 ---
function FruitNode({ id, pos, data, onClick }) {
  return (
    <motion.div 
      className={`absolute ${pos} cursor-pointer group`}
      whileHover={{ scale: 1.2 }}
      onClick={() => onClick(id)}
    >
      <div className="relative flex flex-col items-center">
        {/* 悬浮标签 */}
        <div className="absolute -top-10 opacity-0 group-hover:opacity-100 transition-opacity whitespace-nowrap px-3 py-1 bg-black text-white text-[10px] rounded uppercase tracking-tighter">
          {data.zh}
        </div>
        {/* 图标/果实 */}
        <div className={`p-3 rounded-full shadow-lg bg-white/80 backdrop-blur transition-all group-hover:shadow-xl`}>
          {React.cloneElement(data.icon, { size: 20, strokeWidth: 1.5, className: "text-gray-600" })}
        </div>
        {/* 连线模拟 */}
        <div className="w-px h-8 bg-gray-300 group-hover:bg-gray-400" />
      </div>
    </motion.div>
  );
}# -
