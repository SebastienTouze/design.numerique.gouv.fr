<template>
  <Layout class="mission-page" hideNewsletter showServices>

    <nav role="navigation" class="fr-breadcrumb" aria-label="vous êtes ici :">
      <ol class="fr-breadcrumb__list">
        <li>
          <g-link to="/" class="fr-breadcrumb__link">Accueil</g-link>
        </li>
        <li>
          <g-link to="/commando-ux/" class="fr-breadcrumb__link">Commando UX</g-link>
        </li>
        <li>
          <span aria-current="page">{{ $page.mission.title }}</span>
        </li>
      </ol>
    </nav>

    <div class="cover">
      <div class="cover__container">
        <div class="cover__subhead"><CommandoUX class="cover__subhead-icon" focusable="false" aria-hidden="true"/>Commando UX</div>
        <h1>{{ $page.mission.title }}</h1>
      </div>
    </div>

    <div class="content">

      <section class="mission">
        <div class="mission-detail">
          <span class="mission-detail__icon"><font-awesome :icon="['fas', 'route']" height="16px" aria-hidden="true"/></span>
          <p class="mission-detail__name">Statut de l’intervention :</p>
          <p v-if="$page.mission.status == 'futur'" class="mission-detail__status mission-detail__status--futur">À venir</p>
          <p v-else-if="$page.mission.status == 'present'" class="mission-detail__status mission-detail__status--present">En cours</p>
          <p v-else-if="$page.mission.status == 'past'" class="mission-detail__status mission-detail__status--past">Terminée</p>
        </div>
        <div class="mission-detail">
          <span class="mission-detail__icon"><font-awesome :icon="['fas', 'desktop']" height="16px" aria-hidden="true"/></span>
          <p class="mission-detail__name">Démarche :</p>
          <ul class="mission-detail__content mission-detail__content--procedures">
            <li v-for="procedure in $page.mission.procedures">
              <span v-if="procedure.url"><a :href="procedure.url" :title="procedure.name + ' - Nouvelle fenêtre'" target="_blank">{{ procedure.name }}</a></span>
              <span v-else>{{ procedure.name }}</span>
            </li>
          </ul>
        </div>
        <div v-if="$page.mission.budget" class="mission-detail">
          <span class="mission-detail__icon"><font-awesome :icon="['fas', 'euro-sign']" height="16px" aria-hidden="true"/></span>
          <p class="mission-detail__name">Budget :</p>
          <p class="mission-detail__content">{{ $page.mission.budget }} €</p>
        </div>
        <div class="mission-detail">
          <span class="mission-detail__icon"><font-awesome :icon="['fas', 'user-friends']" height="16px" aria-hidden="true"/></span>
          <p class="mission-detail__name">Impact :</p>
          <p class="mission-detail__content">{{ $page.mission.impact }}</p>
        </div>
        <div v-if="$page.mission.goals.length" class="mission-detail mission-detail--goals">
          <span class="mission-detail__icon"><font-awesome :icon="['fas', 'tasks']" height="16px" aria-hidden="true"/></span>
          <p class="mission-detail__name">Résultats attendus :</p>
          <ol class="mission-detail__content mission-detail__content--goals">
            <li v-for="goal in $page.mission.goals" class="goal">
              <font-awesome v-if="goal.done" class="goal__status goal__status--ok" :icon="['fas', 'check']" width="16" height="16" title="Atteint :" />
              <font-awesome v-else="goal.done" class="goal__status" :icon="['fas', 'check']" width="16" height="16" title="Pas encore atteint :" />
              <span class="goal__name">{{ goal.name }}</span>
            </li>
          </ol>
        </div>
        <div class="mission-detail">
          <span class="mission-detail__icon"><font-awesome :icon="['far', 'calendar-alt']" height="16px" aria-hidden="true"/></span>
          <p class="mission-detail__name">Date de début :</p>
          <p class="mission-detail__content">{{ $page.mission.startDate }}</p>
        </div>
        <div v-if="$page.mission.endDate" class="mission-detail">
          <span class="mission-detail__icon"><font-awesome :icon="['far', 'calendar-check']" height="16px" aria-hidden="true"/></span>
          <p class="mission-detail__name">Date de fin :</p>
          <p class="mission-detail__content">{{ $page.mission.endDate }}</p>
        </div>
        <div v-if="$page.mission.jobs.length" class="mission-detail">
          <span class="mission-detail__icon"><font-awesome :icon="['fas', 'map-marker-alt']" height="16px" aria-hidden="true"/></span>
          <p class="mission-detail__name">Lieu :</p>
          <p class="mission-detail__content">
            <span v-if="$page.mission.direction">{{ $page.mission.department }} — {{ $page.mission.direction }} — {{ $page.mission.place }}</span>
            <span v-else>{{ $page.mission.department }} — {{ $page.mission.place }}</span>
          </p>
        </div>
        <div v-if="$page.mission.team.length" class="mission-detail mission-detail--team">
          <span class="mission-detail__icon"><font-awesome :icon="['fas', 'user-astronaut']" height="16px" aria-hidden="true"/></span>
          <p class="mission-detail__name">Commando déployé :</p>
          <ul v-if="$page.mission.startDate == '7 septembre 2020'" class="mission-detail__content mission-detail__content--team">
            <li v-for="member in $page.mission.team" :key="member.id" class="team-member">
              <g-image :src="member.photo" class="team-member__photo" alt="" />
              <p class="team-member__description">{{ member.firstName }} {{ member.lastName }} - {{ member.job_title }}</p>
            </li>
          </ul>
          <ul v-else class="mission-detail__content mission-detail__content--team">
            <li v-for="member in $page.mission.team" :key="member.id" class="team-member">
              <g-image :src="member.ghostPhoto" class="team-member__photo" alt="" />
              <p class="team-member__description">{{ member.ghostName }}</p>
            </li>
          </ul>
        </div>
        <div v-if="$page.mission.jobs.length" class="mission-detail mission-detail--job">
          <span class="mission-detail__icon"><font-awesome :icon="['fas', 'user-plus']" height="16px" aria-hidden="true"/></span>
          <p class="mission-detail__name">Postes à pourvoir :</p>
          <ul class="mission-detail__content mission-detail__content--job">
            <li v-for="job in $page.mission.jobs" :key="job.id" class="team-member">
              <span class="job__count">{{ job.count }}</span>
              <g-link to="/commando-ux/partager-interet/" class="job__title">
                {{ job.title }}
              </g-link>
            </li>
          </ul>
        </div>
      </section>

      <section>
        <div v-html="$page.mission.content" />
      </section>

    </div>
  </Layout>
</template>


<script>

  import CommandoUX from "~/assets/images/accompagnement.svg";

  export default {
    components: {
      CommandoUX,
    },
    metaInfo() {
      return {
        title: this.$page.mission.title,
        meta: [
          {
            name: "description",
            content: "Une équipe d'experts du Commando UX est mobilisée sur cette mission"
          },
          {
            property: 'og:title',
            content: this.$page.mission.title + " - DesignGouv"
          },
          {
            property: 'og:description',
            content: "Une équipe d'experts du Commando UX est mobilisée sur cette mission"
          },
            {
            property: 'og:image',
            content: "https://design.numerique.gouv.fr/assets/meta-images/CUX.png"
          },
          {
            name: "twitter:card",
            content: "summary_large_image"
          },
          {
            name: "twitter:site",
            content: "@Design_Gouv"
          },
          {
            name: "twitter:title",
            content: this.$page.mission.title + " - DesignGouv"
          },
          {
            name: "twitter:description",
            content: "Une équipe d'experts du Commando UX est mobilisée sur cette mission"
          },
          {
            name: "twitter:image",
            content: "https://design.numerique.gouv.fr/assets/meta-images/CUX.png"
          },
        ]
      };
    }
  };

</script>


<page-query>

  query Mission ($id: ID!) {
    mission: mission (id: $id) {
      title
      status
      procedures {
        name
        url
      }
      budget
      impact
      startDate (format: "D MMMM YYYY", locale : "fr")
      endDate (format: "D MMMM YYYY", locale : "fr")
      department
      direction
      place
      jobs {
        title
        count
      }
      team {
        id
        firstName
        lastName
        photo (width: 52, height: 52, quality: 100)
        job_title
        ghostName
        ghostPhoto (width: 52, height: 52, quality: 100)
      }
      goals {
        name
        done
      }
      content
    }
  }

</page-query>

<style lang="scss">

  @import "src/assets/scss/_vars.scss";

  .mission-page {

    .cover {
      margin-bottom: 4rem;

      h1 {
        font-size: 2.5rem;
      }

      @media only screen and (max-width: $mobile-max-width) {
        margin-bottom: 3rem;

        h1 {
          font-size: 2rem;
        }
      }
    }

    .content {

      max-width: 36rem;
      margin: 0 auto;

      h2 {
        margin-top: 3rem;
      }

      h3 {
        margin-top: 2rem;
      }

      .mission {
        margin-bottom: 4rem;

        .mission-detail {
          display: flex;
          align-items: flex-start;
          justify-content: flex-start;
          margin-bottom: 0.5rem;

          @media only screen and (max-width: $mobile-max-width) {
            flex-wrap: wrap;
          }

          &--team, &--job, &--goals {
            flex-wrap: wrap;
          }

          &__icon {
            background-color: $light-gray;
            border-radius: 50%;
            padding: 0.25rem;
            margin: 0 0.5rem 0 0;
            width: 1.5rem;
            height: 1.5rem;
            color: $red;
            text-align: center;
            flex-shrink: 0;
          }

          &__name {
            flex-shrink: 0;
            margin: 0.25rem 0.5rem 0 0;
            font-weight: bold;
          }

          &__content {
            margin: 0.25rem 0 0 0;

            li {
              &:before {
                content: none;
              }
            }

            &--procedures {
              padding: 0;

              @media only screen and (max-width: $mobile-max-width) {
                margin: 0 0 0 2.5rem;
              }

              li {
                list-style: none;
              }
            }

            &--team, &--job {
              margin: 0.5rem 0 0 2.5rem;
              padding: 0;
              flex-basis: 100%;

              @media only screen and (max-width: $mobile-max-width) {
                margin-top: 0.25rem;
              }

              .team-member, .job {
                display: flex;
                list-style: none;
                margin: 0 0 0.5rem 0;

                &__photo {
                  border-radius: 50%;
                  width: 1.625rem;
                  height: 1.625rem;
                  margin-right: 0.75rem;
                }

                &__count {
                  color: $blue;
                  font-size: 0.875rem;
                  font-weight: bold;
                  background-color: $light-gray;
                  text-align: center;
                  border-radius: 50%;
                  width: 1.375rem;
                  height: 1.375rem;
                  padding: 0.125rem;
                  margin: 0 0.5rem 0 0;
                }

                &__description, &__title {
                    font-weight: normal;
                    margin: 0.075rem 0 0 0;
                }
              }
            }

            &--goals {
              margin: 0.75rem 0 0 2.5rem;
              padding: 0;
              flex-basis: 100%;

              li {
                list-style: none;
              }

              .goal {
                display: flex;
                align-items: center;
                margin-bottom: 0.5rem;

                &__status {
                  margin-right: 0.75rem;
                  font-size: 1rem;
                  border: 2px solid $gray;
                  height: 0.675rem;
                  width: 0.675rem;
                  padding: 0.25rem;
                  border-radius: 50%;
                  color: white;

                  &--ok {
                    color: $blue;
                    border-color: $blue;
                  }
                }

                &__name {
                  font-weight: normal;
                }
              }
            }
          }

          &__status {
            text-align: center;
            width: 4.5rem;
            border: solid 2px $gray;
            border-radius: 0.25rem;
            background-color: $gray;
            margin: 0;
            font-size: 0.875rem;
            font-weight: 500;
            padding: 0.125rem 0.75rem;

            &--futur {
              border-color: $gray;
              color: $black;
              background-color: white;
            }

            &--past {
              border-color: $light-gray;
              color: $black;
              background-color: $light-gray;
            }

            &--present {
              border-color: $blue;
              background-color: white;
              color: $blue;
            }
          }
        }
      }

      .steps {
        margin-left: 56px;
        padding-left: 32px;
        border-left: 4px dotted $blue;

        @media only screen and (max-width: $mobile-max-width) {
          margin-left: 8px;
          padding-left: 21px;
        }

        h3 {
          &:before {
            background-image: url(/assets/images/step.svg);
            background-size: 32px 32px;
            display: inline;
            position: absolute;
            margin-left: -50px;
            margin-top: -5px;
            width: 32px;
            height: 32px;
            content:"";

            @media only screen and (max-width: $mobile-max-width) {
              margin-left: -39px;
              margin-top: -5px;
            }
          }
        }
      }
    }
  }

</style>
