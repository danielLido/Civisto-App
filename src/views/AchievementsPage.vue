<template>
  <ion-page>
    <!-- Header -->
    <ion-header class="ion-no-border">
      <div class="header-container">
        <div class="header-content">
          <ion-button fill="clear" class="back-button" router-link="/tabs/home">
            <ion-icon :icon="arrowBackOutline" slot="icon-only"></ion-icon>
          </ion-button>
          <div class="header-text">
            <h1 class="header-title">Your Superhero Journey</h1>
            <p class="header-subtitle">Celebrate your impact • Unlock new achievements!</p>
          </div>
        </div>
        
        <!-- Segment Tabs -->
        <div class="segment-container">
          <div 
            class="segment-button" 
            :class="{ 'active': activeTab === 'badges' }"
            @click="activeTab = 'badges'"
          >
            Badges
          </div>
          <div 
            class="segment-button" 
            :class="{ 'active': activeTab === 'stats' }"
            @click="activeTab = 'stats'"
          >
            Stats
          </div>
        </div>
      </div>
    </ion-header>

    <ion-content>
      <!-- Achievement Stats Summary -->
      <div class="stats-summary">
        <div class="summary-item">
          <div class="summary-value earned">{{ earnedBadges }}</div>
          <div class="summary-label">Earned</div>
        </div>
        <div class="summary-divider"></div>
        <div class="summary-item">
          <div class="summary-value in-progress">{{ inProgressBadges }}</div>
          <div class="summary-label">In Progress</div>
        </div>
        <div class="summary-divider"></div>
        <div class="summary-item">
          <div class="summary-value total">42</div>
          <div class="summary-label">Total Available</div>
        </div>
      </div>
      
      <!-- Badges Content -->
      <div v-if="activeTab === 'badges'" class="badges-grid">
        <div 
          v-for="badge in achievements.badges" 
          :key="badge.id" 
          class="badge-card"
          :class="{ 'completed': badge.completed }"
        >
          <div 
            class="badge-icon-container" 
            :class="[badge.iconBgClass, {'grayscale': !badge.completed}]"
          >
            <ion-icon :icon="getIconByName(badge.iconName)" class="badge-icon"></ion-icon>
            <div v-if="badge.completed" class="completed-mark">
              <ion-icon :icon="happyOutline" class="completed-icon"></ion-icon>
            </div>
          </div>
          
          <div class="badge-title">{{ badge.title }}</div>
          <div class="badge-description">{{ badge.description }}</div>
          
          <div v-if="badge.completed" class="badge-earned">
            Earned {{ badge.dateEarned }}
          </div>
          <div v-else class="badge-progress">
            <div class="progress-bar">
              <div class="progress-fill" :style="{ width: badge.progress + '%' }"></div>
            </div>
            <div class="progress-text">{{ badge.progress }}% complete</div>
          </div>
        </div>
      </div>
      
      <!-- Stats Content -->
      <div v-if="activeTab === 'stats'" class="stats-container">
        <!-- Stat Items -->
        <div 
          v-for="stat in achievements.stats" 
          :key="stat.id" 
          class="stat-card"
        >
          <div class="stat-icon-container" :class="stat.iconBgClass">
            <ion-icon :icon="getIconByName(stat.iconName)" class="stat-icon"></ion-icon>
          </div>
          <div class="stat-details">
            <div class="stat-title">{{ stat.title }}</div>
            <div class="stat-value">{{ stat.value }}</div>
            <div class="stat-description">{{ stat.description }}</div>
          </div>
        </div>
        
        <!-- Monthly Impact Section -->
        <div class="monthly-impact">
          <h3 class="impact-title">Monthly Impact</h3>
          <div class="impact-card">
            <div class="impact-month">
              <div class="month-header">
                <div class="month-name">April 2025</div>
                <div class="month-value">12 reports</div>
              </div>
              <div class="month-progress-bar">
                <div class="month-progress-fill" style="width: 80%"></div>
              </div>
            </div>
            
            <div class="impact-month">
              <div class="month-header">
                <div class="month-name">March 2025</div>
                <div class="month-value">8 reports</div>
              </div>
              <div class="month-progress-bar">
                <div class="month-progress-fill" style="width: 55%"></div>
              </div>
            </div>
            
            <div class="impact-month">
              <div class="month-header">
                <div class="month-name">February 2025</div>
                <div class="month-value">5 reports</div>
              </div>
              <div class="month-progress-bar">
                <div class="month-progress-fill" style="width: 35%"></div>
              </div>
            </div>
          </div>
        </div>
      </div>
      
      <!-- Next Badge Teaser -->
      <div class="next-badge-container">
        <div class="next-badge-card">
          <div class="next-badge-icon">
            <ion-icon :icon="heartOutline"></ion-icon>
          </div>
          <div class="next-badge-details">
            <div class="next-badge-title">Next Badge: Community Champion</div>
            <div class="next-badge-description">Report issues in 10 different locations to unlock</div>
          </div>
          <div class="next-badge-progress">3/10</div>
        </div>
      </div>
    </ion-content>
  </ion-page>
</template>

<script>
import { defineComponent, ref, computed } from 'vue';
import {
  IonPage,
  IonHeader,
  IonContent,
  IonButton,
  IonIcon,
} from '@ionic/vue';
import {
  arrowBackOutline,
  shieldOutline,
  trophyOutline,
  starOutline,
  flashOutline,
  stopwatchOutline,
  ribbonOutline,
  heartOutline,
  happyOutline
} from 'ionicons/icons';

export default defineComponent({
  name: 'AchievementsPage',
  components: {
    IonPage,
    IonHeader,
    IonContent,
    IonButton,
    IonIcon,
  },
  setup() {
    const activeTab = ref('badges');
    
    // Sample achievements data
    const achievements = {
      badges: [
        {
          id: 1,
          title: "Community Guardian",
          description: "Report issues in 5 different categories",
          iconName: "shieldOutline",
          iconBgClass: "bg-blue",
          progress: 100,
          completed: true,
          dateEarned: "Apr 26, 2025"
        },
        {
          id: 2,
          title: "First Victory",
          description: "Get your first issue resolved",
          iconName: "trophyOutline",
          iconBgClass: "bg-yellow",
          progress: 100,
          completed: true,
          dateEarned: "Apr 15, 2025"
        },
        {
          id: 3,
          title: "Neighborhood Hero",
          description: "Help resolve 10 community issues",
          iconName: "starOutline",
          iconBgClass: "bg-purple",
          progress: 60,
          completed: false,
          dateEarned: null
        },
        {
          id: 4,
          title: "Rapid Responder",
          description: "Report 3 issues within 24 hours",
          iconName: "flashOutline",
          iconBgClass: "bg-orange",
          progress: 33,
          completed: false,
          dateEarned: null
        },
        {
          id: 5,
          title: "Cleanup Champion",
          description: "Report 5 waste management issues",
          iconName: "ribbonOutline",
          iconBgClass: "bg-teal",
          progress: 40,
          completed: false,
          dateEarned: null
        },
        {
          id: 6,
          title: "Road Warrior",
          description: "Get 3 street issues fixed",
          iconName: "ribbonOutline",
          iconBgClass: "bg-red",
          progress: 66,
          completed: false,
          dateEarned: null
        }
      ],
      stats: [
        {
          id: 1,
          title: "City Rank",
          value: "#12",
          description: "Among 5,243 users in your city",
          iconName: "trophyOutline",
          iconBgClass: "bg-yellow"
        },
        {
          id: 2,
          title: "Issues Reported",
          value: "27",
          description: "Top 5% of all reporters",
          iconName: "ribbonOutline",
          iconBgClass: "bg-blue"
        },
        {
          id: 3,
          title: "Successful Fixes",
          value: "16",
          description: "59% success rate",
          iconName: "shieldOutline",
          iconBgClass: "bg-green"
        },
        {
          id: 4,
          title: "Streaks",
          value: "12 days",
          description: "Current reporting streak",
          iconName: "flashOutline",
          iconBgClass: "bg-orange"
        }
      ]
    };
    
    // Count earned and in-progress badges
    const earnedBadges = computed(() => {
      return achievements.badges.filter(badge => badge.completed).length;
    });
    
    const inProgressBadges = computed(() => {
      return achievements.badges.filter(badge => !badge.completed).length;
    });
    
    // Helper function to get the correct icon
    const getIconByName = (iconName) => {
      const icons = {
        shieldOutline,
        trophyOutline,
        starOutline,
        flashOutline,
        stopwatchOutline,
        ribbonOutline,
        heartOutline
      };
      return icons[iconName];
    };
    
    return {
      activeTab,
      achievements,
      earnedBadges,
      inProgressBadges,
      getIconByName,
      
      // Icons
      arrowBackOutline,
      shieldOutline,
      trophyOutline,
      starOutline,
      flashOutline,
      stopwatchOutline,
      ribbonOutline,
      heartOutline,
      happyOutline
    };
  }
});
</script>

<style>
/* Achievements Page Styles */
:root {
  --primary: #22c55e;
  --primary-gradient: linear-gradient(to right, #22c55e, #16a34a);
  --text-dark: #111827;
  --text-medium: #6b7280;
  --text-light: #9ca3af;
  
  --blue-gradient: linear-gradient(to bottom right, #60a5fa, #3b82f6);
  --yellow-gradient: linear-gradient(to bottom right, #fbbf24, #d97706);
  --purple-gradient: linear-gradient(to bottom right, #a78bfa, #7c3aed);
  --orange-gradient: linear-gradient(to bottom right, #fb923c, #ea580c);
  --teal-gradient: linear-gradient(to bottom right, #2dd4bf, #0d9488);
  --red-gradient: linear-gradient(to bottom right, #f87171, #dc2626);
  --green-gradient: linear-gradient(to bottom right, #4ade80, #16a34a);
  
  --next-badge-gradient: linear-gradient(to right, #8b5cf6, #6366f1);
}

/* Header Styles */
.header-container {
  background: var(--primary-gradient);
  padding: 2rem 1rem 1rem 1rem;
  border-bottom-left-radius: 1.5rem;
  border-bottom-right-radius: 1.5rem;
  color: white;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
}

.header-content {
  display: flex;
  align-items: flex-start;
  margin-bottom: 1.5rem;
}

.back-button {
  --color: white;
  --background: rgba(255, 255, 255, 0.2);
  --border-radius: 50%;
  --padding-start: 8px;
  --padding-end: 8px;
  --padding-top: 8px;
  --padding-bottom: 8px;
  height: 36px;
  width: 36px;
  margin: 0 10px 0 0;
}

.header-text {
  flex: 1;
}

.header-title {
  font-size: 1.25rem;
  font-weight: 700;
  margin: 0 0 4px 0;
}

.header-subtitle {
  font-size: 0.875rem;
  opacity: 0.9;
  margin: 0;
}

/* Segment Tabs */
.segment-container {
  display: flex;
  background-color: rgba(255, 255, 255, 0.2);
  border-radius: 0.75rem;
  padding: 4px;
  margin-bottom: 8px;
}

.segment-button {
  flex: 1;
  text-align: center;
  padding: 8px 0;
  border-radius: 0.625rem;
  font-size: 0.875rem;
  font-weight: 500;
  transition: all 0.2s ease;
  cursor: pointer;
}

.segment-button.active {
  background-color: white;
  color: var(--primary);
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.1);
}

/* Stats Summary */
.stats-summary {
  display: flex;
  justify-content: space-between;
  background: white;
  margin: -1rem 1rem 1.5rem 1rem;
  border-radius: 0.75rem;
  padding: 1rem;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.08);
}

.summary-item {
  text-align: center;
  flex: 1;
  padding: 0.25rem 0;
}

.summary-value {
  font-size: 1.25rem;
  font-weight: 700;
  margin-bottom: 0.25rem;
}

.summary-value.earned {
  color: var(--primary);
}

.summary-value.in-progress {
  color: #3b82f6;
}

.summary-value.total {
  color: #8b5cf6;
}

.summary-label {
  font-size: 0.75rem;
  color: var(--text-medium);
}

.summary-divider {
  width: 1px;
  height: 2.5rem;
  background-color: #f3f4f6;
  margin: 0.25rem 0;
}

/* Badges Grid */
.badges-grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 1rem;
  padding: 0 1rem 1rem 1rem;
}

.badge-card {
  background: white;
  border-radius: 1rem;
  padding: 1rem;
  display: flex;
  flex-direction: column;
  align-items: center;
  text-align: center;
  box-shadow: 0 2px 6px rgba(0, 0, 0, 0.05);
  opacity: 0.8;
}

.badge-card.completed {
  opacity: 1;
  border: 1px solid rgba(34, 197, 94, 0.2);
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.08);
}

.badge-icon-container {
  width: 4rem;
  height: 4rem;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  margin-bottom: 0.75rem;
  position: relative;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.15);
}

.badge-icon {
  color: white;
  font-size: 1.75rem;
}

.badge-icon-container.grayscale {
  filter: grayscale(0.8);
  opacity: 0.7;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.completed-mark {
  position: absolute;
  bottom: 0;
  right: 0;
  background: white;
  width: 1.5rem;
  height: 1.5rem;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  border: 2px solid var(--primary);
}

.completed-icon {
  color: var(--primary);
  font-size: 0.875rem;
}

.badge-title {
  font-size: 0.875rem;
  font-weight: 700;
  color: var(--text-dark);
  margin-bottom: 0.25rem;
}

.badge-description {
  font-size: 0.75rem;
  color: var(--text-medium);
  margin-bottom: 0.75rem;
  line-height: 1.3;
}

.badge-earned {
  font-size: 0.75rem;
  color: var(--primary);
  font-weight: 500;
}

.badge-progress {
  width: 100%;
}

.progress-bar {
  height: 0.375rem;
  background-color: #f3f4f6;
  border-radius: 9999px;
  overflow: hidden;
  margin-bottom: 0.25rem;
}

.progress-fill {
  height: 100%;
  background: var(--primary-gradient);
  border-radius: 9999px;
}

.progress-text {
  font-size: 0.75rem;
  color: var(--text-light);
}

/* Stats Styles */
.stats-container {
  padding: 0 1rem;
}

.stat-card {
  background: white;
  border-radius: 1rem;
  padding: 1rem;
  display: flex;
  align-items: center;
  margin-bottom: 1rem;
  box-shadow: 0 2px 6px rgba(0, 0, 0, 0.05);
}

.stat-icon-container {
  width: 4rem;
  height: 4rem;
  border-radius: 0.75rem;
  display: flex;
  align-items: center;
  justify-content: center;
  margin-right: 1rem;
  flex-shrink: 0;
}

.stat-icon {
  color: white;
  font-size: 1.75rem;
}

.stat-details {
  flex: 1;
}

.stat-title {
  font-size: 0.875rem;
  color: var(--text-medium);
  margin-bottom: 0.25rem;
}

.stat-value {
  font-size: 1.5rem;
  font-weight: 700;
  color: var(--text-dark);
  margin-bottom: 0.25rem;
}

.stat-description {
  font-size: 0.75rem;
  color: var(--text-medium);
}

/* Monthly Impact */
.monthly-impact {
  margin-top: 1.5rem;
  margin-bottom: 1.5rem;
}

.impact-title {
  font-size: 0.875rem;
  font-weight: 600;
  color: var(--text-dark);
  margin-bottom: 0.75rem;
}

.impact-card {
  background: white;
  border-radius: 1rem;
  padding: 1rem;
  box-shadow: 0 2px 6px rgba(0, 0, 0, 0.05);
}

.impact-month {
  margin-bottom: 1rem;
}

.impact-month:last-child {
  margin-bottom: 0;
}

.month-header {
  display: flex;
  justify-content: space-between;
  margin-bottom: 0.25rem;
}

.month-name {
  font-size: 0.75rem;
  color: var(--text-medium);
}

.month-value {
  font-size: 0.75rem;
  color: var(--primary);
  font-weight: 500;
}

.month-progress-bar {
  height: 0.625rem;
  background-color: #f3f4f6;
  border-radius: 9999px;
  overflow: hidden;
}

.month-progress-fill {
  height: 100%;
  background: var(--primary-gradient);
  border-radius: 9999px;
}

/* Next Badge Teaser */
.next-badge-container {
  padding: 0 1rem 4rem 1rem;
}

.next-badge-card {
  background: var(--next-badge-gradient);
  border-radius: 1rem;
  padding: 1rem;
  display: flex;
  align-items: center;
  color: white;
  box-shadow: 0 4px 12px rgba(107, 70, 193, 0.2);
}

.next-badge-icon {
  width: 3.5rem;
  height: 3.5rem;
  background-color: rgba(255, 255, 255, 0.2);
  border-radius: 0.75rem;
  display: flex;
  align-items: center;
  justify-content: center;
  margin-right: 1rem;
  flex-shrink: 0;
}

.next-badge-icon ion-icon {
  font-size: 1.75rem;
}

.next-badge-details {
  flex: 1;
}

.next-badge-title {
  font-size: 0.875rem;
  font-weight: 500;
  margin-bottom: 0.25rem;
}

.next-badge-description {
  font-size: 0.75rem;
  opacity: 0.8;
}

.next-badge-progress {
  width: 2.5rem;
  height: 2.5rem;
  background-color: rgba(255, 255, 255, 0.2);
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 0.75rem;
  font-weight: 700;
  margin-left: 0.5rem;
  flex-shrink: 0;
}

/* Background colors for icons */
.bg-blue {
  background: var(--blue-gradient);
}

.bg-yellow {
  background: var(--yellow-gradient);
}

.bg-purple {
  background: var(--purple-gradient);
}

.bg-orange {
  background: var(--orange-gradient);
}

.bg-teal {
  background: var(--teal-gradient);
}

.bg-red {
  background: var(--red-gradient);
}

.bg-green {
  background: var(--green-gradient);
}

/* Responsive adjustments */
@media (min-width: 480px) {
  .badges-grid {
    grid-template-columns: repeat(3, 1fr);
  }
}

@media (min-width: 768px) {
  .badges-grid {
    grid-template-columns: repeat(4, 1fr);
  }
  
  .stats-container, 
  .badges-grid, 
  .next-badge-container,
  .stats-summary {
    max-width: 650px;
    margin-left: auto;
    margin-right: auto;
  }
}

@media (max-width: 359px) {
  .stats-summary {
    padding: 0.75rem 0.5rem;
  }
  
  .summary-value {
    font-size: 1rem;
  }
  
  .badge-icon-container {
    width: 3.5rem;
    height: 3.5rem;
  }
  
  .badge-icon {
    font-size: 1.5rem;
  }
}
</style>