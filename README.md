# _Eaindray Tun_

<img src="https://ahmoor006-Eaindray.github.io/img/green-curry-new-sq-2.jpg" width="50%" align="left">

import React, { useState, useRef } from "react";

// CV Builder - Single-file React component
// Tailwind CSS assumed to be available in the project.
// Features:
// - Left: editable form fields
// - Right: live CV preview
// - Buttons: Load sample (from user's Portfolio), Reset, Print/Export PDF (uses window.print)

export default function CVBuilder() {
  const initialData = {
    fullName: "Miss Eaindray Tun",
    title: "Undergraduate Student — International Business & Management",
    summary:
      "A resilient and determined student from Mon State, Myanmar, studying International Business and Management at Payap University. Passionate about community empowerment, journalism, and business development.",
    contact: {
      email: "[your.email@example.com]",
      phone: "061739041",
      location: "Chiang Mai, Thailand",
    },
    education: [
      {
        degree: "Bachelor (Undergrad) — International Business & Management",
        institution: "Payap International University, Chiang Mai",
        years: "2021 - Present",
        details: "Major: International Business & Management. GPA: 3.11",
      },
      {
        degree: "High School — Mon National Matriculation Exam",
        institution: "Mon State, Myanmar",
        years: "2019 - 2020",
        details: "Completed secondary education; strong interest in media and community work.",
      },
    ],
    skills: [
      "Communication",
      "Research & Reporting",
      "Leadership & Teamwork",
      "Adaptability",
      "Basic Microsoft Office",
    ],
    experience: [
      {
        role: "Volunteer — Live Music Event",
        org: "International College (HIM 343 MICE Operation)",
        years: "Nov 5, 2024",
        details: "Supported event coordination and logistics for a college music event.",
      },
    ],
    certifications: ["Training in Media", "Community Engagement Workshop"],
    languages: ["Burmese (native)", "English (intermediate)", "Thai (basic)"],
    values: ["Respect", "Resilience", "Learning", "Empathy"],
  };

  const [data, setData] = useState(initialData);
  const previewRef = useRef(null);

  function updateField(path, value) {
    setData((d) => {
      const copy = JSON.parse(JSON.stringify(d));
      const keys = path.split(".");
      let cur = copy;
      for (let i = 0; i < keys.length - 1; i++) cur = cur[keys[i]];
      cur[keys[keys.length - 1]] = value;
      return copy;
    });
  }

  function addArrayItem(field) {
    setData((d) => {
      const copy = JSON.parse(JSON.stringify(d));
      copy[field].push( field === 'skills' || field === 'languages' || field === 'certifications' ? "New item" : { role: "New role", org: "Org", years: "Years", details: "Details" } );
      return copy;
    });
  }

  function removeArrayItem(field, idx) {
    setData((d) => {
      const copy = JSON.parse(JSON.stringify(d));
      copy[field].splice(idx, 1);
      return copy;
    });
  }

  function loadPortfolioSample() {
    // Use data already filled from Portfolio.pdf (initialData). This function resets to that.
    setData(initialData);
  }

  function resetEmpty() {
    setData({
      fullName: "",
      title: "",
      summary: "",
      contact: { email: "", phone: "", location: "" },
      education: [],
      skills: [],
      experience: [],
      certifications: [],
      languages: [],
      values: [],
    });
  }

  function handlePrint() {
    // Simple export: open print dialog for the preview area.
    // This will print the whole page; browsers typically allow "Save as PDF".
    window.print();
  }

  return (
    <div className="min-h-screen bg-gray-50 p-6">
      <div className="container mx-auto">
        <h1 className="text-2xl font-semibold mb-4">CV Builder — Live Editor & Preview</h1>

        <div className="flex gap-6">
          {/* Form */}
          <div className="w-1/2 bg-white p-6 rounded-2xl shadow-md">
            <div className="flex gap-3 items-center justify-between mb-4">
              <h2 className="text-lg font-medium">Edit Information</h2>
              <div className="flex gap-2">
                <button
                  className="px-3 py-1 border rounded-md text-sm"
                  onClick={loadPortfolioSample}
                >
                  Load from Portfolio
                </button>
                <button
                  className="px-3 py-1 bg-red-50 border rounded-md text-sm"
                  onClick={resetEmpty}
                >
                  Reset
                </button>
              </div>
            </div>

            <div className="space-y-4">
              <div>
                <label className="block text-sm font-medium">Full name</label>
                <input
                  className="mt-1 block w-full rounded-md border p-2"
                  value={data.fullName}
                  onChange={(e) => setData({ ...data, fullName: e.target.value })}
                />
              </div>

              <div>
                <label className="block text-sm font-medium">Title / One-liner</label>
                <input
                  className="mt-1 block w-full rounded-md border p-2"
                  value={data.title}
                  onChange={(e) => setData({ ...data, title: e.target.value })}
                />
              </div>

              <div>
                <label className="block text-sm font-medium">Summary</label>
                <textarea
                  rows={4}
                  className="mt-1 block w-full rounded-md border p-2"
                  value={data.summary}
                  onChange={(e) => setData({ ...data, summary: e.target.value })}
                />
              </div>

              <div className="grid grid-cols-3 gap-3">
                <div>
                  <label className="block text-sm font-medium">Email</label>
                  <input
                    className="mt-1 block w-full rounded-md border p-2"
                    value={data.contact.email}
                    onChange={(e) => updateField("contact.email", e.target.value)}
                  />
                </div>
                <div>
                  <label className="block text-sm font-medium">Phone</label>
                  <input
                    className="mt-1 block w-full rounded-md border p-2"
                    value={data.contact.phone}
                    onChange={(e) => updateField("contact.phone", e.target.value)}
                  />
                </div>
                <div>
                  <label className="block text-sm font-medium">Location</label>
                  <input
                    className="mt-1 block w-full rounded-md border p-2"
                    value={data.contact.location}
                    onChange={(e) => updateField("contact.location", e.target.value)}
                  />
                </div>
              </div>

              {/* Education */}
              <div>
                <div className="flex items-center justify-between">
                  <label className="block text-sm font-medium">Education</label>
                  <button
                    className="text-sm px-2 py-1 border rounded"
                    onClick={() => addArrayItem("education")}
                  >
                    + Add
                  </button>
                </div>
                <div className="space-y-2 mt-2">
                  {data.education.map((edu, idx) => (
                    <div key={idx} className="border rounded p-2">
                      <div className="grid grid-cols-3 gap-2">
                        <input
                          className="p-1 border rounded"
                          value={edu.degree}
                          onChange={(e) => {
                            const copy = [...data.education];
                            copy[idx].degree = e.target.value;
                            setData({ ...data, education: copy });
                          }}
                        />
                        <input
                          className="p-1 border rounded"
                          value={edu.institution}
                          onChange={(e) => {
                            const copy = [...data.education];
                            copy[idx].institution = e.target.value;
                            setData({ ...data, education: copy });
                          }}
                        />
                        <input
                          className="p-1 border rounded"
                          value={edu.years}
                          onChange={(e) => {
                            const copy = [...data.education];
                            copy[idx].years = e.target.value;
                            setData({ ...data, education: copy });
                          }}
                        />
                      </div>
                      <textarea
                        className="mt-2 p-1 w-full border rounded"
                        rows={2}
                        value={edu.details}
                        onChange={(e) => {
                          const copy = [...data.education];
                          copy[idx].details = e.target.value;
                          setData({ ...data, education: copy });
                        }}
                      />
                      <div className="text-right mt-1">
                        <button
                          className="text-sm text-red-600"
                          onClick={() => removeArrayItem("education", idx)}
                        >
                          Remove
                        </button>
                      </div>
                    </div>
                  ))}
                </div>
              </div>

              {/* Experience */}
              <div>
                <div className="flex items-center justify-between">
                  <label className="block text-sm font-medium">Experience / Activities</label>
                  <button
                    className="text-sm px-2 py-1 border rounded"
                    onClick={() => addArrayItem("experience")}
                  >
                    + Add
                  </button>
                </div>
                <div className="space-y-2 mt-2">
                  {data.experience.map((exp, idx) => (
                    <div key={idx} className="border rounded p-2">
                      <input
                        className="p-1 border rounded w-full mb-1"
                        value={exp.role}
                        onChange={(e) => {
                          const copy = [...data.experience];
                          copy[idx].role = e.target.value;
                          setData({ ...data, experience: copy });
                        }}
                      />
                      <div className="grid grid-cols-2 gap-2">
                        <input
                          className="p-1 border rounded"
                          value={exp.org}
                          onChange={(e) => {
                            const copy = [...data.experience];
                            copy[idx].org = e.target.value;
                            setData({ ...data, experience: copy });
                          }}
                        />
                        <input
                          className="p-1 border rounded"
                          value={exp.years}
                          onChange={(e) => {
                            const copy = [...data.experience];
                            copy[idx].years = e.target.value;
                            setData({ ...data, experience: copy });
                          }}
                        />
                      </div>
                      <textarea
                        rows={2}
                        className="mt-2 p-1 w-full border rounded"
                        value={exp.details}
                        onChange={(e) => {
                          const copy = [...data.experience];
                          copy[idx].details = e.target.value;
                          setData({ ...data, experience: copy });
                        }}
                      />
                      <div className="text-right mt-1">
                        <button
                          className="text-sm text-red-600"
                          onClick={() => removeArrayItem("experience", idx)}
                        >
                          Remove
                        </button>
                      </div>
                    </div>
                  ))}
                </div>
              </div>

              {/* Skills / Certifications / Languages */}
              <div className="grid grid-cols-3 gap-4">
                <div>
                  <label className="block text-sm font-medium">Skills</label>
                  <div className="mt-1 space-y-1">
                    {data.skills.map((s, i) => (
                      <div key={i} className="flex gap-2">
                        <input
                          className="flex-1 p-1 border rounded"
                          value={s}
                          onChange={(e) => {
                            const copy = [...data.skills];
                            copy[i] = e.target.value;
                            setData({ ...data, skills: copy });
                          }}
                        />
                        <button className="text-red-600" onClick={() => removeArrayItem("skills", i)}>
                          x
                        </button>
                      </div>
                    ))}
                    <button className="mt-2 text-sm border rounded px-2 py-1" onClick={() => addArrayItem("skills")}>
                      + Add skill
                    </button>
                  </div>
                </div>

                <div>
                  <label className="block text-sm font-medium">Certifications</label>
                  <div className="mt-1 space-y-1">
                    {data.certifications.map((c, i) => (
                      <div key={i} className="flex gap-2">
                        <input
                          className="flex-1 p-1 border rounded"
                          value={c}
                          onChange={(e) => {
                            const copy = [...data.certifications];
                            copy[i] = e.target.value;
                            setData({ ...data, certifications: copy });
                          }}
                        />
                        <button className="text-red-600" onClick={() => removeArrayItem("certifications", i)}>
                          x
                        </button>
                      </div>
                    ))}
                    <button className="mt-2 text-sm border rounded px-2 py-1" onClick={() => addArrayItem("certifications")}>
                      + Add
                    </button>
                  </div>
                </div>

                <div>
                  <label className="block text-sm font-medium">Languages</label>
                  <div className="mt-1 space-y-1">
                    {data.languages.map((l, i) => (
                      <div key={i} className="flex gap-2">
                        <input
                          className="flex-1 p-1 border rounded"
                          value={l}
                          onChange={(e) => {
                            const copy = [...data.languages];
                            copy[i] = e.target.value;
                            setData({ ...data, languages: copy });
                          }}
                        />
                        <button className="text-red-600" onClick={() => removeArrayItem("languages", i)}>
                          x
                        </button>
                      </div>
                    ))}
                    <button className="mt-2 text-sm border rounded px-2 py-1" onClick={() => addArrayItem("languages")}>
                      + Add
                    </button>
                  </div>
                </div>
              </div>

              <div className="flex justify-end gap-2 mt-4">
                <button className="px-4 py-2 border rounded" onClick={() => window.scrollTo({ top: 0, behavior: 'smooth' })}>
                  Back to Top
                </button>
                <button className="px-4 py-2 bg-blue-600 text-white rounded" onClick={handlePrint}>
                  Export / Print CV
                </button>
              </div>
            </div>
          </div>

          {/* Preview */}
          <div className="w-1/2">
            <div className="bg-white p-6 rounded-2xl shadow-md" ref={previewRef}>
              <div className="flex items-start gap-4">
                <div>
                  <div className="text-2xl font-bold">{data.fullName || "Full name"}</div>
                  <div className="text-sm text-gray-600">{data.title || "Professional title"}</div>
                  <div className="mt-3 text-sm">
                    {data.contact.email} • {data.contact.phone} • {data.contact.location}
                  </div>
                </div>
              </div>

              <hr className="my-4" />

              <section>
                <h3 className="font-semibold">Summary</h3>
                <p className="text-sm mt-1">{data.summary || "Short summary about yourself."}</p>
              </section>

              <div className="grid grid-cols-2 gap-6 mt-4">
                <section>
                  <h3 className="font-semibold">Education</h3>
                  <div className="mt-2 space-y-3">
                    {data.education.map((edu, idx) => (
                      <div key={idx}>
                        <div className="text-sm font-medium">{edu.degree}</div>
                        <div className="text-sm text-gray-600">{edu.institution} • {edu.years}</div>
                        <div className="text-sm mt-1">{edu.details}</div>
                      </div>
                    ))}
                    {data.education.length === 0 && <div className="text-sm text-gray-500">No education entered.</div>}
                  </div>
                </section>

                <section>
                  <h3 className="font-semibold">Skills & Languages</h3>
                  <div className="mt-2 text-sm">
                    <div className="mb-2">
                      <div className="font-medium text-sm">Skills</div>
                      <div className="flex flex-wrap gap-2 mt-1">
                        {data.skills.map((s, i) => (
                          <span key={i} className="text-xs px-2 py-1 border rounded-full">{s}</span>
                        ))}
                        {data.skills.length === 0 && <div className="text-gray-500">No skills entered.</div>}
                      </div>
                    </div>

                    <div>
                      <div className="font-medium text-sm">Languages</div>
                      <div className="mt-1 text-sm">
                        {data.languages.join(" • ")}
                        {data.languages.length === 0 && <div className="text-gray-500">No languages entered.</div>}
                      </div>
                    </div>
                  </div>
                </section>
              </div>

              <section className="mt-4">
                <h3 className="font-semibold">Experience & Activities</h3>
                <div className="mt-2 space-y-3 text-sm">
                  {data.experience.map((exp, idx) => (
                    <div key={idx}>
                      <div className="font-medium">{exp.role}</div>
                      <div className="text-gray-600 text-sm">{exp.org} • {exp.years}</div>
                      <div className="mt-1">{exp.details}</div>
                    </div>
                  ))}
     
