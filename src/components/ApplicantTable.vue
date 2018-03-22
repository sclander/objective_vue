<template>
  <div id="page">
    <table class="job-applicants">
      <thead>
        <tr>
          <th v-for="header in headers" :key="header">{{ header }}</th>
        </tr>
      </thead>
      <tbody>
        <template v-for="job in formattedData.jobs">
          <template v-for="(applicant, $index_a) in job.applicants">
            <tr v-for="(skill, $index_s) in applicant.skills" :key="skill.id">
              <td v-if="$index_a === 0 && $index_s === 0" :rowSpan="job.totalSkills">{{ job.name }}</td>
              <template v-if="$index_s === 0">
                <td :rowSpan="applicant.skills.length">{{ applicant.name }}</td>
                <td :rowSpan="applicant.skills.length">{{ applicant.email }}</td>
                <td :rowSpan="applicant.skills.length">{{ applicant.website }}</td>
                <td>{{ applicant.skills[0]['name']}}</td>
                <td :rowSpan="applicant.skills.length">{{ applicant.cover_letter }}</td>
              </template>
              <td v-if="$index_s !== 0">{{ applicant.skills[$index_s]['name'] }}</td>
            </tr>
          </template>
        </template>
      </tbody>
      <tfoot>
        <tr>
          <td colspan="6">{{totalApplicants}} Applicants, {{uniqueSkills}} Unique Skills</td>
        </tr>
      </tfoot>
    </table>
  </div>
</template>

<script>
  export default {
    methods: {
      // Translate the JSON data to a structure that works better with v-for
      reformatData(data) {
        let formattedData = { jobs:[] };
        const jobs = data.jobs;
        const applicants = data.applicants;
        const skills = data.skills;

        // Create base object for each job id
        for (let job of jobs) {
          let newJob = {
            id: job.id,
            name: job.name,
            applicants: [],
            totalSkills: 0
          }

          // Find applicants for job id and create base object for each match
          for (let applicant of applicants) {
            if (applicant.job_id === job.id) {
              let newApplicant = {
                id: applicant.id,
                name: applicant.name,
                email: applicant.email,
                website: applicant.website,
                cover_letter: applicant.cover_letter,
                skills: []
              };

              // Find matching skills and add them to applicant's skills array
              for (let skill of skills) {
                if (skill.applicant_id === applicant.id) {
                  let newSkill = {
                    id: skill.id,
                    name: skill.name
                  }

                  // Add skill to applicant array and increment job skill counter
                  newApplicant.skills.push(newSkill);
                  newJob.totalSkills++;
                }
              }

              // Add applicant to job array
              newJob.applicants.push(newApplicant);
            }
          }
          // Add job to main object array
          formattedData.jobs.push(newJob);
        }
        return formattedData;
      }
    },
    data() {
      return {
        // Static data for header names
        headers: [ 
          'Job', 
          'Applicant Name', 
          'Email Address', 
          'Website', 
          'Skills', 
          'Cover Letter Paragraph'
        ],
        // Placeholders for transformed JSON data
        formattedData: { jobs: [] }
      }
    },
    computed: {
      // Calculate total number of applicants
      totalApplicants() {
        let count = 0;

        // If jobs.length === 0, data has not yet been loaded
        if (this.formattedData && this.formattedData.jobs.length > 0) {
          for (let job of this.formattedData.jobs) {
            count += job.applicants.length;
          }
        }

        return count;
      },
      // Calculate total number of unique skills
      uniqueSkills() {
        let count = 0;
        let skillList = [];

        // If jobs.length === 0, data has not yet been loaded
        if (this.formattedData && this.formattedData.jobs.length > 0) {
          for (let job of this.formattedData.jobs) {
            for (let applicant of job.applicants) {
              for(let skill of applicant.skills) {
                // Check for existing name key to eliminate duplicates
                if (skillList.indexOf(skill.name) === -1) {
                  skillList.push(skill.name);
                  count++;
                }
              }
            }
          }
        }

        return count;
      }
    },
    props: ['applicantData'],
    watch: {
      // Update formatted data on change to raw applicantData JSON
      applicantData(newVal) {
        this.formattedData = this.reformatData(newVal);
      }
    }
  }
</script>